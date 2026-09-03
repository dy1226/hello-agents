# Chapter 1 - 第一个 ReAct Agent

## 📁 文件说明

### 核心文件
- **FirstAgentTest.py** - ReAct Agent 主程序（Python 脚本）
- **FirstAgentTest.ipynb** - ReAct Agent 交互式版本（Jupyter Notebook）
- **.env** - 你的 API 配置（已配置好）

### 配置文件
- **.env.example** - 配置模板
- **API_KEY_SETUP.md** - API 配置详细说明

### 入门文档
- **START_HERE.md** - Chapter 1 完整入门指南
- **FIX_COMPLETED.md** - 环境修复说明（已完成）
- **run_simple.bat** - 一键运行脚本

---

## 🚀 快速开始

### 方法 1：直接运行 Python 脚本（推荐）

```powershell
cd d:\code\agent\hello-agents\code\chapter1
python FirstAgentTest.py
```

### 方法 2：使用批处理文件

```powershell
.\run_simple.bat
```

### 方法 3：使用 Jupyter Notebook（交互式）

```powershell
jupyter notebook FirstAgentTest.ipynb
```

---

## 🎯 学习重点

这个 Chapter 展示了 **ReAct（Reasoning + Acting）Agent** 的核心概念：

1. **Thought（思考）** - Agent 分析当前情况
2. **Action（行动）** - 调用工具或给出答案
3. **Observation（观察）** - 获取工具执行结果
4. **循环迭代** - 重复上述过程直到完成任务

---

## 📖 推荐阅读顺序

1. **START_HERE.md** - 理解 ReAct Agent 的工作原理
2. **FirstAgentTest.py** - 阅读代码实现
3. 运行程序，观察 Agent 的思考过程
4. 修改用户输入，测试不同场景

---

## 🔧 修改测试场景

编辑 `FirstAgentTest.py` 第 193 行：

```python
# 简单查询
user_prompt = "今天上海的天气怎么样？"

# 复杂任务
user_prompt = "我想去杭州旅游，帮我查天气并推荐景点"

# 测试边界
user_prompt = "帮我查一下不存在的城市的天气"
```

---

## ✅ 环境已配置完成

- ✅ Python 3.12.7 (Miniconda)
- ✅ 所有依赖已安装
- ✅ API 配置正确
- ✅ 可以直接运行

需要帮助请查看 `FIX_COMPLETED.md`
