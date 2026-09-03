## 🚨 重要：你需要配置真实的 API 密钥！

当前 `.env` 文件中的配置是**占位符**，需要替换成真实的 API Key。

---

## ⚡ 快速配置步骤

### 步骤 1：获取 API 密钥

#### 方案 A：Deepseek（推荐，最快最便宜）

1. **访问**：https://platform.deepseek.com/
2. **注册**：
   - 点击右上角"注册/登录"
   - 使用邮箱注册（国内邮箱即可）
3. **获取 API Key**：
   - 登录后进入控制台
   - 点击左侧"API Keys"
   - 点击"Create API Key"
   - 复制生成的密钥（格式类似：`sk-abc123def456...`）
4. **充值**（可选）：
   - 新用户可能有免费额度
   - 或充值最少金额（10元左右）测试

#### 方案 B：通义千问（如果你有阿里云账号）

1. **访问**：https://dashscope.aliyun.com/
2. **登录**：使用阿里云账号
3. **获取 API Key**：
   - 进入控制台
   - 找到 API Key 管理
   - 创建并复制密钥

#### 方案 C：Moonshot（月之暗面）

1. **访问**：https://platform.moonshot.cn/
2. **注册并获取 API Key**

---

### 步骤 2：编辑 .env 文件

```bash
# 在 PowerShell 中运行
notepad d:\code\agent\hello-agents\code\chapter1\.env
```

**修改前（当前状态）**：
```env
API_KEY=sk-xxxxxxxxxxxxxx
BASE_URL=https://api.deepseek.com/v1
MODEL_ID=deepseek-chat
```

**修改后（以 Deepseek 为例）**：
```env
API_KEY=sk-abc123def456ghi789jkl012mno345pqr678stu901vwx234yz
BASE_URL=https://api.deepseek.com/v1
MODEL_ID=deepseek-chat
```

⚠️ **注意**：
- 把 `sk-xxxxxxxxxxxxxx` 替换成你从平台复制的**完整密钥**
- 不要有多余的空格
- 不要添加引号
- 保存文件（Ctrl+S）

---

### 步骤 3：验证配置

```bash
# 测试 API 连接
python -c "from openai import OpenAI; import os; from dotenv import load_dotenv; load_dotenv(); client = OpenAI(api_key=os.getenv('API_KEY'), base_url=os.getenv('BASE_URL')); response = client.chat.completions.create(model=os.getenv('MODEL_ID'), messages=[{'role': 'user', 'content': '你好'}]); print('✅ API 配置成功！'); print('响应:', response.choices[0].message.content)"
```

如果看到 `✅ API 配置成功！` 和模型回复，说明配置正确。

---

### 步骤 4：运行 Agent

```bash
python FirstAgentTest.py
```

---

## 🎯 如果你现在还没有 API 密钥

### 选项 1：先用 Demo 模式（我帮你改代码）

如果你想先看看效果，我可以帮你修改代码，使用模拟数据运行：

```python
# 修改 FirstAgentTest.py，模拟 LLM 响应
# 这样可以先理解 Agent 的运行流程
```

### 选项 2：等待配置好 API 后再运行

这是推荐的方式，因为真实的 API 调用才能体验完整的 Agent 能力。

---

## 📝 配置示例对比

### ❌ 错误配置（当前）
```env
API_KEY=sk-xxxxxxxxxxxxxx          # ❌ 这是占位符
BASE_URL=https://api.deepseek.com/v1
MODEL_ID=deepseek-chat
```

### ✅ 正确配置（Deepseek）
```env
API_KEY=sk-1234567890abcdefghijklmnopqrstuvwxyz1234567890  # ✅ 真实密钥
BASE_URL=https://api.deepseek.com/v1
MODEL_ID=deepseek-chat
```

### ✅ 正确配置（通义千问）
```env
API_KEY=sk-abcd1234efgh5678ijkl9012mnop3456              # ✅ 真实密钥
BASE_URL=https://dashscope.aliyuncs.com/compatible-mode/v1
MODEL_ID=qwen-turbo
```

### ✅ 正确配置（Moonshot）
```env
API_KEY=sk-xyz789abc456def123ghi890                      # ✅ 真实密钥
BASE_URL=https://api.moonshot.cn/v1
MODEL_ID=moonshot-v1-8k
```

---

## 🔍 如何判断配置是否正确

### 正确的 API Key 特征：
- ✅ 以 `sk-` 开头
- ✅ 后面是一串长的随机字符（通常40-60个字符）
- ✅ 只包含字母和数字，没有特殊符号
- ✅ 示例：`sk-1a2b3c4d5e6f7g8h9i0j1k2l3m4n5o6p7q8r9s0t`

### 错误的配置：
- ❌ `sk-xxxxxxxxxxxxxx` - 这是占位符
- ❌ `your_api_key_here` - 这是占位符
- ❌ `<your-api-key>` - 这是占位符
- ❌ 空白或未填写

---

## 💰 成本说明

### Deepseek 费用（2024年价格参考）
- **输入**：约 ¥0.001 / 1000 tokens
- **输出**：约 ¥0.002 / 1000 tokens
- **运行一次示例**：约 ¥0.01-0.02（1-2分钱）
- **充值 10元**：可以运行几百次

### 免费额度
- 部分平台新用户有免费额度
- 通义千问对个人开发者较为友好

---

## 🆘 常见问题

### Q: 我注册了但没收到验证邮件
**A**: 检查垃圾邮件箱，或使用其他邮箱

### Q: 充值需要多少钱？
**A**: Deepseek 最低充值通常 10元起，足够测试用

### Q: 能不能免费使用？
**A**: 
- 部分平台有免费额度（如通义千问）
- 或者我可以帮你修改代码，用模拟数据演示流程

### Q: 配置好了还是没输出
**A**: 运行：
```bash
python FirstAgentTest.py 2>&1 | Out-File output.txt
type output.txt
```
查看详细错误信息

---

## 🎯 你现在应该做什么？

### 如果你想立即体验（推荐）：

1. **注册 Deepseek 账号**（5分钟）
2. **获取 API Key**（1分钟）
3. **编辑 .env 文件**（1分钟）
4. **运行程序**（1分钟）

**总计：8分钟搞定！**

### 如果暂时不想注册：

**告诉我**，我可以：
1. 修改代码使用模拟数据演示
2. 或者提供一个简化版本，先理解原理

---

## 📖 下一步

配置好 API Key 后：
1. 运行 `python FirstAgentTest.py`
2. 观察 Agent 的思考过程
3. 阅读 `LEARNING_GUIDE.md` 继续学习

需要帮助随时问我！🚀
