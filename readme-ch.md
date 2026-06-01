# Rethlas

Rethlas 是一个基于两个 Codex 智能体构建的数学自然语言推理系统：

- **生成智能体 (generation agent)**：从 Markdown 文件中读取数学问题，并编写非正式的证明蓝图（proof blueprint）。
- **验证智能体 (verification agent)**：检查证明蓝图，生成结构化的判决结果，并作为生成智能体的验证器。

预期的部署顺序如下：

1. 将验证智能体作为本地 HTTP 服务启动。
2. 通过 Codex 运行生成智能体。
3. 让生成智能体在“证明与修复”循环中调用验证服务。

## 仓库布局

- `agents/generation`：证明生成智能体
- `agents/verification`：证明验证智能体

具体而言：
- 原始问题存放在 `agents/generation/data/` 下，例如未分类的问题 `agents/generation/data/example.md`，或已分类的问题 `agents/generation/data/modrep/modrep.md`、`agents/generation/data/example/example1.md`。
- 用于在静态网站中渲染结果的 Zola 项目位于 `agents/generation/site/`。

## 1. 安装 Codex CLI

安装 Codex CLI：

```bash
npm install -g @openai/codex
```

## 2. 克隆仓库

```bash
git clone https://github.com/frenzymath/Rethlas.git
cd Rethlas
```

## 3. 启动验证服务

```bash
cd agents/verification
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
uvicorn api.server:app --host 0.0.0.0 --port 8091
```

使用 uv：
```bash
cd agents/verification
uv venv 
uv pip install -r requirements.txt
uv run uvicorn api.server:app --host 0.0.0.0 --port 8091
```

## 4. 在包含的示例上运行生成智能体

```bash
cd agents/generation
python3 -m venv .venv
source .venv/bin/activate
pip install -r mcp/requirements.txt
./tests/run_example.sh
```

该脚本会：

- 读取 `agents/generation/data/example.md`
- 在 `agents/generation` 内部运行 `codex exec`
- 将运行日志写入 `agents/generation/logs/example/example.md`
- 将内存工件写入 `agents/generation/memory/example/`
- 将证明草案写入 `agents/generation/results/example/blueprint.md`
- 如果验证成功，将验证后的证明写入 `agents/generation/results/example/blueprint_verified.md`

## 5. 运行你自己的问题

将你的问题放在 `agents/generation/data/` 下的一个 Markdown 文件中。保存为：

```text
agents/generation/data/my_problem.md
```

然后运行：

```bash
cd agents/generation
source .venv/bin/activate
PROBLEM_FILE=data/my_problem.md ./tests/run_example.sh
```

你可以在 `data/` 下创建子目录对问题进行分组，生成的工件将保留该结构。例如：

```bash
PROBLEM_FILE=data/modrep/modrep.md ./tests/run_example.sh
```

要为问题附加用户提供的参考资料，请创建一个具有相同前缀的同级参考目录：

```text
agents/generation/data/modrep/modrep.refs/
```

当该目录存在时，生成智能体在进行外部搜索之前会先读取其中的文件。
参考文件可以是 Markdown、LaTeX、纯文本或 PDF，但相比 PDF，更推荐使用 Markdown、LaTeX 和纯文本。实际上，PDF 在智能体运行前会被转换为 `.extracted/` 下的提取文本。

## 6. 在浏览器中查看结果

- `agents/generation/site`：用于在浏览器中浏览结果的 Zola 站点

结果是包含 LaTeX 数学公式的 Markdown 文件。为了正确渲染它们，仓库中包含了一个使用 [MATbook](https://www.getzola.org/themes/matbook/) 主题的本地 [Zola](https://www.getzola.org/) 站点。

### 前置条件

安装 Zola。

可以通过终端中的包管理器轻松安装 Zola。例如，在 Mac 上运行：

```bash
brew install zola
```

在 ArchLinux 上运行：

```bash
sudo pacman -S zola
```

对于其他操作系统，请参阅 [Zola 安装文档](https://www.getzola.org/documentation/getting-started/installation/)。

### 服务启动

在 `agents/generation/` 目录下：

```bash
./site/serve.sh
```

首次运行时，这会自动克隆 [MATbook](https://www.getzola.org/themes/matbook/) 主题。然后，它会将 `results/` 中的所有结果同步到站点并启动本地服务器。在浏览器中打开 http://localhost:3264。

`agents/generation/data/your_category` 中的每个问题将成为 `your_category` 章节中的一个小节，而直接位于 `agents/generation/data` 中的问题将归入 `unclassified` 章节。

### 更新 MATbook 主题

```bash
./site/setup_theme.sh
```

这将从 [MATbook 仓库](https://github.com/srliu3264/MATbook) 拉取最新版本。
