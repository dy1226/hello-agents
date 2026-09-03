# 🎯 Chapter 1 快速启动指南

## ✅ 问题已解决！

我已经为你创建了 `.env` 和 `.env.example` 文件。

## 📝 现在需要做的事情

### 1. 配置你的 API 密钥

打开 `d:\code\agent\hello-agents\code\chapter1\.env` 文件，填入你的配置：

```env
# 🤖 LLM API 配置（必需）
API_KEY=sk-144e08550f9f40e1a559178f5c9248a9
BASE_URL=https://api.deepseek.com
MODEL_ID=deepseek-v4-flash

# 🔍 Tavily 搜索 API（可选）
TAVILY_API_KEY=tvly-你的真实密钥
```

### 2. 获取 API 密钥的方法

#### Deepseek（推荐）
- 访问：https://platform.deepseek.com/
- 注册账号
- 进入"API Keys"页面
- 创建新的 API Key
- 复制密钥到 `.env` 文件

#### 通义千问（备选）
- 访问：https://dashscope.aliyun.com/
- 阿里云账号登录
- 获取 API Key
- 修改 `.env` 文件：
  ```env
  API_KEY=sk-你的密钥
  BASE_URL=https://dashscope.aliyuncs.com/compatible-mode/v1
  MODEL_ID=qwen-turbo
  ```

#### Moonshot（备选）
- 访问：https://platform.moonshot.cn/
- 注册获取 API Key
- 修改 `.env` 文件：
  ```env
  API_KEY=sk-你的密钥
  BASE_URL=https://api.moonshot.cn/v1
  MODEL_ID=moonshot-v1-8k
  ```

#### Tavily 搜索（可选）
- 访问：https://tavily.com/
- 免费注册
- 获取 API Key
- 添加到 `.env` 文件

> **注意**：如果暂时没有 Tavily Key，可以先运行程序，景点推荐功能会报错但不影响天气查询功能。

### 3. 运行你的第一个 Agent

```bash
# 确保在正确的目录
cd d:\code\agent\hello-agents\code\chapter1

# 运行脚本
python FirstAgentTest.py
```

### 4. 预期输出

```
用户输入: 你好，请帮我查询一下今天北京的天气，然后根据天气推荐一个合适的旅游景点。
========================================
--- 循环 1 ---

正在调用大语言模型...
大语言模型响应成功。
模型输出:
Thought: 我需要先查询北京的天气
Action: get_weather(city="北京")

Observation: 北京当前天气：Clear，气温15摄氏度
========================================
...
```

## 🔧 常见问题

### Q1: 提示 "API_KEY" 环境变量未找到

**原因**：`.env` 文件配置不正确

**解决**：
1. 确认 `.env` 文件在 `chapter1` 目录下
2. 确认已填入真实的 API Key（去掉 `sk-xxxxxx` 占位符）
3. 确认没有多余的空格或注释符号 `#`

### Q2: API 调用失败

**原因**：网络问题或 API Key 无效

**解决**：
```bash
# 测试 API 连接
python -c "from openai import OpenAI; client = OpenAI(api_key='你的key', base_url='你的url'); print('连接成功')"
```

### Q3: Tavily 错误

**原因**：没有配置 Tavily API Key

**解决方案 1**：获取免费的 Tavily Key
**解决方案 2**：暂时注释掉景点推荐功能，只测试天气查询

### Q4: 模块导入失败

```bash
# 安装缺失的依赖
pip install openai requests python-dotenv tavily-python
```

## 🎯 快速测试（无需 Tavily）

如果你只想快速测试天气查询功能：

1. 修改 `FirstAgentTest.py` 第 154 行：
```python
# 修改前
user_prompt = "你好，请帮我查询一下今天北京的天气，然后根据天气推荐一个合适的旅游景点。"

# 修改后（只查天气）
user_prompt = "你好，请帮我查询一下今天北京的天气。"
```

2. 修改系统提示词（第 1-17 行）：
```python
AGENT_SYSTEM_PROMPT = """
你是一个智能旅行助手。你的任务是分析用户的请求，并使用可用工具一步步地解决问题。

# 可用工具:
- `get_weather(city: str)`: 查询指定城市的实时天气。

# 输出格式要求:
你的每次回复必须严格遵循以下格式，包含一对Thought和Action：

Thought: [你的思考过程和下一步计划]
Action: [你要执行的具体行动]

Action的格式必须是以下之一：
1. 调用工具：function_name(arg_name="arg_value")
2. 结束任务：Finish[最终答案]

请开始吧！
"""
```

## 📖 下一步

成功运行后，你可以：
1. ✅ 修改城市参数，查询其他城市
2. ✅ 添加新的工具函数
3. ✅ 修改 Prompt，改变 Agent 行为
4. ✅ 查看 `LEARNING_GUIDE.md` 了解完整学习路径

## 🆘 需要帮助？

如果遇到问题：
1. 检查 `.env` 配置
2. 查看终端错误信息
3. 参考本文档的"常见问题"部分
4. 在项目 Issues 提问：https://github.com/datawhalechina/hello-agents/issues

祝你学习顺利！🚀
