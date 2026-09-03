# FirstAgentTest.py 代码详解

## 📚 目录
1. [整体架构](#整体架构)
2. [核心概念](#核心概念)
3. [代码逐段解析](#代码逐段解析)
4. [关键技术点](#关键技术点)
5. [运行流程图](#运行流程图)

---

## 整体架构

这是一个实现了 **ReAct (Reasoning + Acting)** 模式的智能 Agent，它能够：
- 理解用户需求
- 规划执行步骤
- 调用外部工具
- 根据结果推理
- 给出最终答案

```
用户输入 → Agent思考 → 调用工具 → 观察结果 → 继续思考 → ... → 完成任务
```

---

## 核心概念

### ReAct 循环的三个阶段

1. **Thought（思考）** - Agent 分析当前情况，决定下一步做什么
2. **Action（行动）** - 执行具体操作（调用工具或结束任务）
3. **Observation（观察）** - 获取行动的结果，作为下一轮思考的输入

---

## 代码逐段解析

### 第一部分：系统提示词（System Prompt）

```python
AGENT_SYSTEM_PROMPT = """
你是一个智能旅行助手。你的任务是分析用户的请求，并使用可用工具一步步地解决问题。

# 可用工具:
- `get_weather(city: str)`: 查询指定城市的实时天气。
- `get_attraction(city: str, weather: str)`: 根据城市和天气搜索推荐的旅游景点。

# 输出格式要求:
你的每次回复必须严格遵循以下格式，包含一对Thought和Action：

Thought: [你的思考过程和下一步计划]
Action: [你要执行的具体行动]
...
"""
```

**作用：**
- 定义 Agent 的角色和能力范围
- 告诉 LLM 有哪些工具可用
- **最关键**：规定了输出格式（Thought-Action 结构）

**为什么重要：**
这是 Prompt Engineering 的核心。通过精确的格式要求，让 LLM 输出可解析的结构化文本，而不是自由发挥。

---

### 第二部分：工具函数定义

#### 工具 1：天气查询

```python
def get_weather(city: str) -> str:
    """通过调用 wttr.in API 查询真实的天气信息。"""
    url = f"https://wttr.in/{city}?format=j1"
    
    try:
        response = requests.get(url)
        response.raise_for_status() 
        data = response.json()
        
        current_condition = data['current_condition'][0]
        weather_desc = current_condition['weatherDesc'][0]['value']
        temp_c = current_condition['temp_C']
        
        return f"{city}当前天气：{weather_desc}，气温{temp_c}摄氏度"
        
    except requests.exceptions.RequestException as e:
        return f"错误：查询天气时遇到网络问题 - {e}"
    except (KeyError, IndexError) as e:
        return f"错误：解析天气数据失败，可能是城市名称无效 - {e}"
```

**关键点：**
1. **真实 API 调用**：调用 wttr.in 获取真实天气数据
2. **错误处理**：区分网络错误和数据解析错误
3. **自然语言输出**：返回易读的文本，而不是原始 JSON

**设计思想：**
- Agent 的工具应该返回自然语言，而不是结构化数据
- 让 LLM 能够直接理解工具的返回结果

#### 工具 2：景点推荐

```python
def get_attraction(city: str, weather: str) -> str:
    """根据城市和天气，使用Tavily Search API搜索并返回优化后的景点推荐。"""
    api_key = os.environ.get("TAVILY_API_KEY")
    
    if not api_key:
        return "错误：未配置TAVILY_API_KEY。"
    
    tavily = TavilyClient(api_key=api_key)
    query = f"'{city}' 在'{weather}'天气下最值得去的旅游景点推荐及理由"
    
    try:
        response = tavily.search(query=query, search_depth="basic", include_answer=True)
        
        if response.get("answer"):
            return response["answer"]
        
        # 处理原始搜索结果...
        
    except Exception as e:
        return f"错误：执行Tavily搜索时出现问题 - {e}"
```

**关键点：**
1. **AI 搜索引擎**：Tavily 是专门为 AI Agent 设计的搜索 API
2. **构造精确查询**：将城市和天气信息组合成搜索关键词
3. **include_answer=True**：让 Tavily 返回总结性答案，而不是原始搜索结果

#### 工具注册表

```python
available_tools = {
    "get_weather": get_weather,
    "get_attraction": get_attraction,
}
```

**作用：**
- 将工具名称（字符串）映射到实际函数
- 允许动态调用工具：`available_tools[tool_name](**kwargs)`

---

### 第三部分：LLM 客户端

```python
class OpenAICompatibleClient:
    """一个用于调用任何兼容OpenAI接口的LLM服务的客户端。"""
    def __init__(self, model: str, api_key: str, base_url: str):
        self.model = model
        self.client = OpenAI(api_key=api_key, base_url=base_url)

    def generate(self, prompt: str, system_prompt: str) -> str:
        """调用LLM API来生成回应。"""
        messages = [
            {'role': 'system', 'content': system_prompt},
            {'role': 'user', 'content': prompt}
        ]
        response = self.client.chat.completions.create(
            model=self.model,
            messages=messages,
            stream=False
        )
        return response.choices[0].message.content
```

**关键点：**
1. **OpenAI 兼容接口**：可以切换不同的 LLM 提供商（OpenAI、DeepSeek、本地模型等）
2. **System Prompt 分离**：将 Agent 的系统指令与用户对话历史分开
3. **简洁封装**：隐藏 API 调用细节

---

### 第四部分：主循环（最关键！）

#### 初始化

```python
user_prompt = "你好，请帮我查询一下今天北京的天气，然后根据天气推荐一个合适的旅游景点。"
prompt_history = [f"用户请求: {user_prompt}"]
```

**prompt_history** 是整个 Agent 的"记忆"：
- 记录所有的 Thought、Action、Observation
- 每次调用 LLM 时都传入完整历史
- 让 Agent 能够基于之前的结果继续推理

#### ReAct 主循环

```python
for i in range(5):  # 最大循环 5 次
    print(f"--- 循环 {i+1} ---\n")
    
    # 步骤 1: 构建完整 Prompt
    full_prompt = "\n".join(prompt_history)
    
    # 步骤 2: 调用 LLM 进行思考
    llm_output = llm.generate(full_prompt, system_prompt=AGENT_SYSTEM_PROMPT)
    
    # 步骤 2.5: 截断多余的 Thought-Action 对
    match = re.search(r'(Thought:.*?Action:.*?)(?=\n\s*(?:Thought:|Action:|Observation:)|\Z)', 
                      llm_output, re.DOTALL)
    if match:
        truncated = match.group(1).strip()
        if truncated != llm_output.strip():
            llm_output = truncated
            print("已截断多余的 Thought-Action 对")
    
    print(f"模型输出:\n{llm_output}\n")
    prompt_history.append(llm_output)
    
    # 步骤 3: 解析 Action
    action_match = re.search(r"Action: (.*)", llm_output, re.DOTALL)
    if not action_match:
        observation = "错误: 未能解析到 Action 字段。请确保你的回复严格遵循 'Thought: ... Action: ...' 的格式。"
        observation_str = f"Observation: {observation}"
        print(f"{observation_str}\n" + "="*40)
        prompt_history.append(observation_str)
        continue
    
    action_str = action_match.group(1).strip()
    
    # 步骤 4: 检查是否结束
    if action_str.startswith("Finish"):
        final_answer = re.match(r"Finish\[(.*)\]", action_str).group(1)
        print(f"任务完成，最终答案: {final_answer}")
        break
    
    # 步骤 5: 解析工具调用
    tool_name = re.search(r"(\w+)\(", action_str).group(1)
    args_str = re.search(r"\((.*)\)", action_str).group(1)
    kwargs = dict(re.findall(r'(\w+)="([^"]*)"', args_str))
    
    # 步骤 6: 执行工具
    if tool_name in available_tools:
        observation = available_tools[tool_name](**kwargs)
    else:
        observation = f"错误：未定义的工具 '{tool_name}'"
    
    # 步骤 7: 记录观察结果
    observation_str = f"Observation: {observation}"
    print(f"{observation_str}\n" + "="*40)
    prompt_history.append(observation_str)
```

---

## 关键技术点

### 1. 正则表达式解析

Agent 使用正则表达式从 LLM 输出中提取结构化信息：

```python
# 提取 Action
action_match = re.search(r"Action: (.*)", llm_output, re.DOTALL)

# 提取工具名称
tool_name = re.search(r"(\w+)\(", action_str).group(1)

# 提取参数
kwargs = dict(re.findall(r'(\w+)="([^"]*)"', args_str))
```

**示例：**
输入：`Action: get_weather(city="北京")`
- `tool_name` = `"get_weather"`
- `kwargs` = `{"city": "北京"}`

### 2. 历史记录累积

```python
prompt_history = [
    "用户请求: 查询北京的天气...",
    "Thought: 我需要先查天气\nAction: get_weather(city='北京')",
    "Observation: 北京当前天气：Sunny，气温34摄氏度",
    "Thought: 已获得天气，接下来推荐景点\nAction: get_attraction(...)",
    ...
]
```

每次循环都把整个历史传给 LLM，形成连贯的推理链。

### 3. 循环终止条件

两种方式退出循环：
1. **正常结束**：LLM 输出 `Action: Finish[答案]`
2. **超时保护**：最多循环 5 次（防止无限循环）

### 4. 错误处理

三层错误处理：
1. **工具内部**：网络错误、数据解析错误
2. **Action 解析**：格式不正确时提示 LLM
3. **工具不存在**：调用未定义的工具时报错

---

## 运行流程图

```
开始
  ↓
用户输入: "查询北京天气并推荐景点"
  ↓
[循环 1]
  LLM 思考: "我需要先查天气"
  LLM 行动: get_weather(city="北京")
  执行工具 → 观察: "北京天气 Sunny，34°C"
  ↓
[循环 2]
  LLM 思考: "已知天气，现在推荐景点"
  LLM 行动: get_attraction(city="北京", weather="Sunny")
  执行工具 → 观察: "推荐颐和园..."
  ↓
[循环 3]
  LLM 思考: "已获取所有信息，可以回答了"
  LLM 行动: Finish[北京今天晴天34°C，推荐去颐和园]
  ↓
结束 → 输出最终答案
```

---

## 实践建议

### 修改练习

1. **添加新工具**
   ```python
   def get_restaurant(city: str, cuisine: str) -> str:
       # 实现餐厅推荐
       pass
   
   available_tools["get_restaurant"] = get_restaurant
   ```
   记得在 AGENT_SYSTEM_PROMPT 中也添加工具说明！

2. **修改用户输入**
   ```python
   user_prompt = "我想去上海旅游3天，帮我规划一下"
   ```

3. **增加循环次数**
   ```python
   for i in range(10):  # 处理更复杂的任务
   ```

### 调试技巧

1. **打印完整历史**
   ```python
   print("\n".join(prompt_history))
   ```

2. **查看原始 LLM 输出**
   已经包含在代码中：`print(f"模型输出:\n{llm_output}\n")`

3. **测试单个工具**
   ```python
   print(get_weather("北京"))
   ```

---

## 总结

这个 Agent 的核心是：
1. **Prompt Engineering**：通过系统提示词规范 LLM 的输出格式
2. **正则解析**：从自然语言中提取结构化信息
3. **循环推理**：让 LLM 能够多步思考和行动
4. **工具调用**：将 LLM 的输出转化为实际的函数调用

理解了这些，你就掌握了 Agent 开发的基础！
