# 🚀 Hello-Agents 快速启动手册

## 立即开始：5 分钟运行你的第一个 Agent

### Step 1: 配置环境变量

```bash
# 进入 chapter1 目录
cd d:\code\agent\hello-agents\code\chapter1

# 编辑 .env 文件（我已经为你创建好了），填入你的 API 密钥
notepad .env  # Windows
# nano .env  # Linux/Mac
```

`.env` 文件内容示例：
```env
# 使用 Deepseek（推荐，性价比高）
API_KEY=sk-xxxxxxxxxxxxxx
BASE_URL=https://api.deepseek.com/v1
MODEL_ID=deepseek-chat

# 或使用通义千问
# API_KEY=sk-xxxxxxxxxxxxxx
# BASE_URL=https://dashscope.aliyuncs.com/compatible-mode/v1
# MODEL_ID=qwen-turbo

# 或使用 Moonshot
# API_KEY=sk-xxxxxxxxxxxxxx
# BASE_URL=https://api.moonshot.cn/v1
# MODEL_ID=moonshot-v1-8k

# Tavily 搜索 API（可选，用于联网搜索）
TAVILY_API_KEY=tvly-xxxxxxxxxxxxxx
```

### Step 2: 安装依赖

```bash
pip install openai requests python-dotenv tavily-python
```

### Step 3: 运行第一个 Agent

```bash
cd d:\code\agent\hello-agents\code\chapter1
python FirstAgentTest.py
```

### 预期输出

```
用户输入: 你好，请帮我查询一下今天北京的天气，然后根据天气推荐一个合适的旅游景点。
========================================
--- 循环 1 ---

正在调用大语言模型...
大语言模型响应成功。
模型输出:
Thought: 我需要先查询北京的天气，然后根据天气情况推荐合适的旅游景点。
Action: get_weather(city="北京")

Observation: 北京当前天气：Clear，气温15摄氏度
========================================
--- 循环 2 ---

正在调用大语言模型...
大语言模型响应成功。
模型输出:
Thought: 北京天气晴朗，温度适宜，适合户外活动。我将推荐适合晴天游玩的景点。
Action: get_attraction(city="北京", weather="Clear")

Observation: 在晴朗的天气下，北京最值得去的景点包括故宫、长城和颐和园...
========================================
--- 循环 3 ---

正在调用大语言模型...
大语言模型响应成功。
模型输出:
Thought: 我已经获取了天气和景点信息，可以给出最终答案了。
Action: Finish[北京今天天气晴朗，气温15°C，非常适合户外游览。推荐您去故宫或颐和园...]

任务完成，最终答案: 北京今天天气晴朗，气温15°C...
```

---

## 🎯 理解你刚刚运行的 Agent

### 核心概念：ReAct 循环

```
用户输入 → Thought（思考）→ Action（行动）→ Observation（观察）→ 循环...
```

### 代码关键部分解析

```python
# 1. 定义可用工具
available_tools = {
    "get_weather": get_weather,      # 查询天气
    "get_attraction": get_attraction, # 推荐景点
}

# 2. Agent 循环执行
for i in range(5):  # 最多循环 5 次
    # 2.1 调用 LLM 思考
    llm_output = llm.generate(full_prompt, AGENT_SYSTEM_PROMPT)
    
    # 2.2 解析 Action
    action_str = parse_action(llm_output)
    
    # 2.3 执行工具
    if tool_name in available_tools:
        observation = available_tools[tool_name](**kwargs)
    
    # 2.4 记录观察结果，进入下一轮
    prompt_history.append(f"Observation: {observation}")
```

---

## 🔧 常见问题快速修复

### 问题 1：API 调用失败

**症状**：`调用LLM API时发生错误`

**解决方案**：
```bash
# 检查 API Key 是否正确
# 检查网络连接
# 确认 BASE_URL 格式正确（必须以 /v1 结尾）

# 测试连接
python -c "from openai import OpenAI; client = OpenAI(api_key='your_key', base_url='your_url'); print('连接成功')"
```

### 问题 2：Tavily API 错误

**症状**：`错误：未配置TAVILY_API_KEY`

**解决方案**：
```python
# 方式 1：在 .env 文件中配置
TAVILY_API_KEY=tvly-xxxxxx

# 方式 2：暂时注释掉搜索功能
# 修改 FirstAgentTest.py，只使用天气查询工具
```

### 问题 3：模块导入失败

**症状**：`ModuleNotFoundError: No module named 'xxx'`

**解决方案**：
```bash
# 创建虚拟环境（推荐）
python -m venv venv
venv\Scripts\activate  # Windows
# source venv/bin/activate  # Linux/Mac

# 重新安装依赖
pip install openai requests python-dotenv tavily-python
```

---

## 📝 动手练习：修改你的第一个 Agent

### 练习 1：修改城市

```python
# 在 FirstAgentTest.py 中修改
user_prompt = "请帮我查询上海的天气，并推荐景点"
```

### 练习 2：添加新工具

```python
# 添加一个查询餐厅的工具
def get_restaurant(city: str, cuisine: str) -> str:
    """查询指定城市的餐厅推荐"""
    return f"{city}推荐的{cuisine}餐厅：..."

# 注册工具
available_tools["get_restaurant"] = get_restaurant

# 更新 System Prompt
AGENT_SYSTEM_PROMPT = """
你是一个智能旅行助手。

# 可用工具:
- get_weather(city: str): 查询天气
- get_attraction(city: str, weather: str): 推荐景点
- get_restaurant(city: str, cuisine: str): 推荐餐厅

...
"""
```

### 练习 3：优化 Prompt

```python
# 让 Agent 更简洁
AGENT_SYSTEM_PROMPT = """
...
# 输出要求:
- 思考过程简洁明了
- 最终答案包含：天气、景点、出行建议
- 使用友好的语气
"""
```

---

## 🎓 下一步学习路径

### 路线 1：深入框架（推荐新手）

```bash
# 安装 HelloAgents 框架
pip install helloagents

# 运行框架示例
cd d:\code\agent\hello-agents\code\chapter7
python test_simple_agent.py
```

### 路线 2：学习经典范式

```bash
cd d:\code\agent\hello-agents\code\chapter4

# ReAct 范式
python ReAct.py

# Plan-and-Solve 范式
python Plan_and_solve.py

# Reflection 范式
python Reflection.py
```

### 路线 3：实战项目

```bash
# 智能旅行助手（完整项目）
cd d:\code\agent\hello-agents\code\chapter13\helloagents-trip-planner
# 查看 README.md 了解详细说明
```

---

## 📚 重要文件位置

```
d:\code\agent\hello-agents\
├── docs/                          # 文档（理论知识）
│   ├── chapter1/                  # 第一章：初识智能体
│   ├── chapter4/                  # 第四章：经典范式
│   └── chapter7/                  # 第七章：构建框架
│
├── code/                          # 代码（实践项目）
│   ├── chapter1/                  # 第一个 Agent
│   │   ├── FirstAgentTest.py      # ⭐ 从这里开始
│   │   └── FirstAgentTest.ipynb   # Jupyter 版本
│   │
│   ├── chapter4/                  # 经典范式实现
│   │   ├── ReAct.py               # ReAct 范式
│   │   ├── Plan_and_solve.py      # Plan-and-Solve
│   │   └── Reflection.py          # Reflection
│   │
│   ├── chapter7/                  # 自研框架
│   │   ├── my_simple_agent.py     # 简单 Agent
│   │   └── test_*.py              # 测试文件
│   │
│   ├── chapter8/                  # 记忆系统
│   │   └── 01_MemoryTool_*.py     # 记忆工具示例
│   │
│   └── chapter13/                 # 完整项目：旅行助手
│       └── helloagents-trip-planner/
│
└── LEARNING_GUIDE.md              # ⭐ 完整学习指南
```

---

## 💡 学习技巧

### 1. 使用 Jupyter Notebook（推荐）

```bash
pip install jupyter
cd d:\code\agent\hello-agents\code\chapter1
jupyter notebook FirstAgentTest.ipynb
```

**优势**：
- 可以分步运行
- 方便调试和实验
- 保存运行结果

### 2. 打印中间状态

```python
# 在循环中添加调试信息
print(f"当前 Prompt 历史: {prompt_history}")
print(f"解析的 Action: {action_str}")
print(f"工具返回: {observation}")
```

### 3. 修改循环次数

```python
# 测试时可以增加循环次数
for i in range(10):  # 原来是 5
    ...
```

### 4. 保存对话历史

```python
# 保存完整对话
with open('agent_log.txt', 'w', encoding='utf-8') as f:
    for msg in prompt_history:
        f.write(msg + '\n\n')
```

---

## 🎯 第一天学习目标

- [x] 配置好开发环境
- [x] 成功运行 FirstAgentTest.py
- [x] 理解 Thought-Action-Observation 循环
- [x] 修改城市参数并重新运行
- [x] 阅读 `docs/chapter1/第一章 初识智能体.md`

---

## 🔗 快速链接

- **完整学习指南**：`d:\code\agent\LEARNING_GUIDE.md`
- **项目 README**：`d:\code\agent\hello-agents\README.md`
- **在线文档**：https://datawhalechina.github.io/hello-agents/
- **问题反馈**：https://github.com/datawhalechina/hello-agents/issues

---

## 🎉 恭喜！

你已经完成了第一个 Agent 的运行！

**接下来可以**：
1. 查看 `LEARNING_GUIDE.md` 了解完整学习路径
2. 尝试修改代码，添加新功能
3. 阅读 chapter1 文档理解原理
4. 加入社区交流群，和其他学习者交流

**记住**：Agent 开发是实践驱动的，多动手、多实验！🚀
