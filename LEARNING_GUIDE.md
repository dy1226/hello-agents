# Hello-Agents 学习实操指南

## 🚀 快速开始：你的第一个 Agent

### 阶段一：环境搭建与第一个 Agent（预计 1-2 天）

#### 1. 安装依赖

```bash
# 进入项目目录
cd d:\code\agent\hello-agents

# 安装基础依赖
pip install openai requests python-dotenv tavily-python
```

#### 2. 配置 API 密钥

创建 `.env` 文件（参考 `code/chapter1/.env.example`）：

```env
# LLM API 配置（选择你能访问的服务）
API_KEY=your_api_key_here
BASE_URL=your_base_url_here
MODEL_ID=your_model_id_here

# Tavily 搜索 API（用于联网搜索）
TAVILY_API_KEY=your_tavily_key_here
```

**API 获取建议**：
- LLM：可以用 OpenAI、通义千问、Deepseek、Moonshot 等任意兼容 OpenAI 格式的服务
- Tavily：访问 https://tavily.com 免费注册获取 API Key

#### 3. 运行第一个 Agent

```bash
# 方式1：直接运行 Python 脚本
cd code/chapter1
python FirstAgentTest.py

# 方式2：使用 Jupyter Notebook（推荐）
jupyter notebook FirstAgentTest.ipynb
```

**这个 Agent 做什么？**
- 查询城市天气（真实 API）
- 根据天气推荐旅游景点（联网搜索）
- 展示完整的 ReAct 思维链（Thought → Action → Observation）

---

### 阶段二：安装自研框架 HelloAgents（预计 1 天）

#### 1. 安装 HelloAgents 框架

```bash
# 安装稳定版本（推荐）
pip install helloagents

# 或安装开发版本
pip install git+https://github.com/jjyaoao/helloagents.git
```

#### 2. 运行框架示例

```bash
cd code/chapter7

# 测试简单 Agent
python test_simple_agent.py

# 测试 ReAct Agent
python test_react_agent.py

# 测试带计算器工具的 Agent
python test_my_calculator.py
```

**HelloAgents 框架特点**：
- 基于 OpenAI 原生 API
- 轻量级、易扩展
- 支持工具调用、记忆系统、流式输出
- 完全开源，可以学习框架设计

---

### 阶段三：记忆系统实践（预计 2-3 天）

#### 运行记忆系统示例

```bash
cd code/chapter8

# 1. 基础记忆操作
python 01_MemoryTool_Basic_Operations.py

# 2. 记忆系统架构理解
python 02_MemoryTool_Architecture.py

# 3. 工作记忆实现
python 03_WorkingMemory_Implementation.py

# 4. RAG 集成
python 04_RAGTool_MarkItDown_Pipeline.py
```

**重点理解**：
- 四种记忆类型：工作记忆、情景记忆、语义记忆、感知记忆
- 记忆的添加、搜索、整合、遗忘机制
- RAG（检索增强生成）的实际应用

---

### 阶段四：上下文工程（预计 2 天）

```bash
cd code/chapter9

# 1. 上下文构建基础
python 01_context_builder_basic.py

# 2. Agent 集成上下文
python 02_context_builder_with_agent.py

# 3. 笔记工具操作
python 03_note_tool_operations.py

# 4. 终端工具示例
python 05_terminal_tool_examples.py
```

---

### 阶段五：完整项目实战（预计 1 周）

#### 项目 1：智能旅行助手（推荐）

```bash
cd code/chapter13/helloagents-trip-planner

# 后端
cd backend
pip install -r requirements.txt
python run.py

# 前端（新终端）
cd frontend
npm install
npm run dev
```

**项目亮点**：
- 前后端分离架构
- 集成高德地图 API
- 多智能体协作（规划、查询、推荐）
- 实时交互界面

#### 项目 2：赛博小镇（高级）

```bash
cd code/chapter15/Helloagents-AI-Town

# 后端
cd backend
pip install -r requirements.txt
python main.py

# 前端（Godot 游戏引擎）
# 需要安装 Godot 4.x，然后打开项目
```

**项目亮点**：
- 多 Agent 社会模拟
- 记忆与关系系统
- 游戏化呈现

---

## 📚 学习路径建议

### 路径 A：快速上手型（2 周完成核心内容）

```
Week 1:
Day 1-2: 第一章（FirstAgentTest） + 环境配置
Day 3-4: 第四章（ReAct/Reflection 范式实现）
Day 5-7: 第七章（自研框架核心代码）

Week 2:
Day 1-3: 第八章（记忆系统）
Day 4-5: 第九章（上下文工程）
Day 6-7: 第十三章（旅行助手项目）
```

### 路径 B：系统学习型（1-2 个月）

```
第 1-2 周：基础理论 + 第一个 Agent
- 阅读第 1-3 章文档
- 运行 chapter1 所有示例
- 理解 Transformer、Prompt Engineering

第 3-4 周：经典范式实现
- 第四章：手写 ReAct、Plan-and-Solve、Reflection
- 第五章：体验 Dify/Coze 等低代码平台
- 第六章：学习 LangGraph/AutoGen 框架

第 5-6 周：自研框架
- 第七章：深入 HelloAgents 源码
- 实现自己的 Agent/Tool/LLM 类
- 理解框架设计模式

第 7-8 周：高级特性
- 第八章：记忆与 RAG
- 第九章：上下文工程
- 第十章：MCP/A2A 协议

第 9-12 周：综合项目
- 第十三章：旅行助手
- 第十四章：深度研究 Agent
- 第十五章：赛博小镇
- 第十六章：毕业设计（自己的项目）
```

---

## 🎓 关键学习建议

### 1. 代码优先，理论辅助
- **不要**只看文档不动手
- **务必**每个示例都跑一遍
- **建议**修改参数、添加功能来深化理解

### 2. 循序渐进
```
最小 Agent 单元 → 添加工具 → 添加记忆 → 多轮对话 → 多 Agent 协作
```

### 3. 重点文件清单

**必须理解的核心代码**：
```
chapter1/FirstAgentTest.py          # Agent 的本质
chapter4/ReAct.py                   # 经典范式
chapter7/my_simple_agent.py         # 框架基础
chapter7/my_react_agent.py          # ReAct 实现
chapter8/01_MemoryTool_*.py         # 记忆系统
chapter13/                          # 完整项目架构
```

### 4. 常见问题速查

**Q: API 调用失败？**
```python
# 检查 .env 配置
# 确认 API Key 有效
# 测试网络连接
python -c "from openai import OpenAI; print('OK')"
```

**Q: 导入 hello_agents 失败？**
```bash
# 确认安装
pip show helloagents

# 重新安装
pip install --upgrade helloagents
```

**Q: 依赖冲突？**
```bash
# 创建虚拟环境（强烈推荐）
python -m venv venv
# Windows
venv\Scripts\activate
# Linux/Mac
source venv/bin/activate

# 在虚拟环境中安装
pip install -r requirements.txt
```

---

## 🔥 实战练习建议

### 练习 1：改造第一个 Agent
- 添加"查询餐厅推荐"工具
- 实现多城市对比功能
- 优化 Prompt，提升回答质量

### 练习 2：构建自己的工具
```python
# 参考 chapter7/my_calculator_tool.py
# 实现一个新工具，比如：
- 翻译工具（调用翻译 API）
- 数据库查询工具
- 文件操作工具
```

### 练习 3：毕业设计方向
- **代码审查 Agent**：自动分析代码质量
- **学习助手 Agent**：个性化学习路径规划
- **数据分析 Agent**：自动生成数据报告
- **客服 Agent**：智能问答 + 工单系统

---

## 📖 推荐阅读顺序

### 文档阅读
1. `docs/前言.md` - 了解项目背景
2. `docs/chapter1/` - Agent 基本概念
3. `docs/chapter4/` - 经典范式原理
4. `docs/chapter7/` - 框架设计思想
5. `docs/chapter8/` - 记忆系统设计
6. `Extra-Chapter/` - 社区经验分享

### 代码阅读
1. `chapter1/FirstAgentTest.py` - 最小可用 Agent
2. `chapter7/my_*.py` - 框架核心组件
3. `chapter8/01_*.py` - 记忆系统实现
4. `chapter13/backend/app/agents/` - 完整项目架构

---

## 🎯 学习检查点

### ✅ 阶段一完成标志
- [ ] 成功运行 FirstAgentTest.py
- [ ] 理解 Thought-Action-Observation 循环
- [ ] 能修改 Prompt 改变 Agent 行为

### ✅ 阶段二完成标志
- [ ] 安装并使用 HelloAgents 框架
- [ ] 创建自定义 Tool
- [ ] 理解 Agent/Tool/LLM 三层架构

### ✅ 阶段三完成标志
- [ ] 实现带记忆的 Agent
- [ ] 理解四种记忆类型
- [ ] 集成 RAG 功能

### ✅ 毕业标志
- [ ] 完成一个完整的多 Agent 项目
- [ ] 理解 Agent 设计模式
- [ ] 能独立设计 Agent 系统架构

---

## 🔗 重要资源

- **项目主页**：https://github.com/datawhalechina/hello-agents
- **在线文档**：https://datawhalechina.github.io/hello-agents/
- **HelloAgents 框架**：https://github.com/jjyaoao/helloagents
- **问题反馈**：在 GitHub Issues 提问
- **社区交流**：扫描 README 中的读者群二维码

---

## 💡 最后的建议

1. **不要追求完美**：先跑通，再优化
2. **勤做笔记**：记录遇到的问题和解决方案
3. **多做实验**：修改代码看看会发生什么
4. **参与社区**：分享你的学习笔记和项目
5. **持续关注**：Agent 技术发展很快，保持学习

**记住**：构建 Agent 是一个迭代过程，从简单到复杂，从单一到多元。
先让它"能跑"，再让它"跑得好"，最后让它"跑得优雅"。

祝你学习顺利！🚀
