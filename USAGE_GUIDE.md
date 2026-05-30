# Rethlas 项目使用指导

Rethlas 是一个专注于数学研究级问题的自然语言推理系统。它由两个核心代理组成：**生成代理 (Generation Agent)** 负责编写证明蓝图，**验证代理 (Verification Agent)** 负责对证明进行严格的逻辑审查。

---

## 1. 环境准备

在开始之前，请确保您的系统已安装以下工具：

- **Node.js**: 用于安装和运行 Codex CLI。
- **Python 3.10+**: 用于运行验证服务和相关脚本。
- **Zola**: 用于在本地浏览器中查看渲染后的证明结果。
- **poppler (pdftotext)**: (可选) 如果需要处理 PDF 格式的参考资料。

### 安装 Codex CLI
```bash
npm install -g @openai/codex
```

### 安装 Zola (以 macOS 为例)
```bash
brew install zola
```

---

## 2. 快速开始

项目的标准运行流程分为两步：启动验证服务，然后运行生成代理。

### 第一步：启动验证服务
验证服务必须在生成代理运行之前启动，因为它充当了生成代理的“审稿人”。

```bash
cd agents/verification
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
# 默认端口 8091
uvicorn api.server:app --host 0.0.0.0 --port 8091
```

### 第二步：运行生成代理
生成代理将读取数学问题，并通过与验证服务的交互，逐步完善证明。

```bash
cd agents/generation
python3 -m venv .venv
source .venv/bin/activate
pip install -r mcp/requirements.txt
./tests/run_example.sh
```

运行完成后，结果将保存在 `agents/generation/results/` 目录下。

---

## 3. 进阶使用

### 运行自定义数学问题
1. 将您的数学问题（Markdown 格式）放入 `agents/generation/data/` 目录下。例如：`data/my_problem.md`。
2. 运行以下命令：
   ```bash
   cd agents/generation
   source .venv/bin/activate
   PROBLEM_FILE=data/my_problem.md ./tests/run_example.sh
   ```

### 使用本地参考资料
如果您有相关的定理或论文（Markdown, LaTeX, TXT 或 PDF），可以将其放入与问题同名的 `.refs` 文件夹中：
- 问题文件：`data/algebra/modrep.md`
- 参考目录：`data/algebra/modrep.refs/`

系统在生成证明时会优先读取这些参考资料。

---

## 4. 查看渲染结果

项目集成了一个本地静态站点生成器 (Zola)，可以将 Markdown 格式的证明渲染成美观的数学网页。

```bash
cd agents/generation
./site/serve.sh
```
然后在浏览器中打开 [http://localhost:3264](http://localhost:3264)。

---

## 5. 核心配置

您可以根据需要通过环境变量调整模型行为：

- `MODEL`: 使用的 Codex 模型（默认 `gpt-5.4`）。
- `REASONING_EFFORT`: 推理强度（默认 `xhigh`）。
- `CODEX_BIN`: Codex 可执行文件路径（默认 `codex`）。

示例：
```bash
MODEL=gpt-4o ./tests/run_example.sh
```

---

## 6. 项目结构说明

- `agents/generation/`: 生成代理的逻辑、技能和本地数据。
  - `data/`: 存放原始数学问题。
  - `memory/`: 存放推理过程中的中间记忆记录。
  - `results/`: 最终生成的证明蓝图。
- `agents/verification/`: 验证代理的逻辑和 API 服务。
  - `api/server.py`: 验证服务的 FastAPI 实现。
- `README.md`: 项目的基础安装说明。
- `USAGE_GUIDE.md`: 本使用指导文件。
