# Rethlas 项目工作原理

**Rethlas** 是一个**数学证明自动生成与验证系统**，采用双智能体（Agent）架构，由 OpenAI Codex CLI 作为推理引擎进行编排。

---

## 整体架构

```
                    ┌──────────────────────────────────┐
                    │        Codex CLI (推理引擎)        │
                    │   基于 AGENTS.md 指令驱动行为      │
                    └──────────┬───────────────────────┘
                               │
              ┌────────────────┼────────────────┐
              ▼                │                ▼
    ┌─────────────────┐        │      ┌─────────────────┐
    │  生成智能体       │   HTTP  │      │  验证智能体       │
    │  (Generation)    │──POST──▶│      │  (Verification)  │
    │                  │ /verify │      │                  │
    │  编写证明草稿      │◀────────│      │  检查逻辑正确性    │
    │  迭代修复         │  JSON   │      │  输出结构化判决    │
    └─────────────────┘        │      └─────────────────┘
                               │
                     MCP 工具服务器 (FastMCP)
                     提供定理搜索、记忆管理
```

---

## 目录结构

```
Rethlas/
├── README.md                          # 英文项目文档
├── USAGE_GUIDE.md                     # 中文使用指南
├── agents/
│   ├── generation/                    # 生成智能体
│   │   ├── AGENTS.md                  # 智能体指令文档（225行）
│   │   ├── data/                      # 数学问题 markdown 文件
│   │   │   ├── example.md             # "素数阶有限群必为循环群"
│   │   │   ├── example/
│   │   │   │   ├── example1.md        # "p² 阶群必有非平凡中心"
│   │   │   │   └── example2.md        # "p 幂阶群必有非平凡中心"
│   │   │   └── modrep/
│   │   │       ├── modrep.md          # 模表示论问题
│   │   │       └── modrep.refs/       # 用户提供的参考文献
│   │   ├── mcp/
│   │   │   ├── server.py              # MCP 工具服务器（6个工具）
│   │   │   └── requirements.txt
│   │   ├── memory/                    # 持久化推理记忆
│   │   ├── results/                   # 生成的证明（blueprint.md + blueprint_verified.md）
│   │   ├── logs/                      # Codex 运行日志
│   │   ├── downloads/                 # 下载的论文/参考文献
│   │   ├── site/                      # Zola 静态站点（结果浏览）
│   │   │   ├── config.toml
│   │   │   ├── serve.sh               # 站点启动脚本
│   │   │   ├── setup_theme.sh         # 主题安装脚本
│   │   │   ├── transform_math.py      # LaTeX→MathJax 预处理
│   │   │   └── templates/index.html
│   │   └── tests/
│   │       └── run_example.sh         # 主入口脚本
│   └── verification/                  # 验证智能体
│       ├── AGENTS.md                  # 验证指令文档（171行）
│       ├── api/
│       │   ├── server.py              # FastAPI 服务（localhost:8091）
│       │   └── requirements.txt
│       ├── mcp/
│       │   ├── server.py              # MCP 工具服务器（6个工具）
│       │   └── requirements.txt
│       ├── schemas/
│       │   └── verification_output.schema.json  # 输出 JSON Schema
│       ├── memory/                    # 验证运行记忆
│       ├── results/                   # 验证输出（verification.json）
│       ├── scripts/
│       │   └── test_verify_endpoint.py
│       └── requirements.txt
```

---

## 核心组件

### 1. 生成智能体（Generation Agent）

**位置**：[`agents/generation/`](agents/generation/)

**入口**：[`tests/run_example.sh`](agents/generation/tests/run_example.sh) 脚本启动，核心命令：

```bash
codex exec -C agents/generation -m gpt-5.4 --config model_reasoning_effort=xhigh <prompt>
```

**行为定义**：[`AGENTS.md`](agents/generation/AGENTS.md)（225行指令文档），定义了完整的工作流程：

- **输入**：`data/` 目录下的数学问题 markdown 文件
- **输出**：
  - 工作草稿：`results/{problem_id}/blueprint.md`
  - 验证通过的证明：`results/{problem_id}/blueprint_verified.md`

**`problem_id` 规则**：问题文件路径相对于 `data/`，去掉 `.md` 后缀，保留分类目录。例如：
- `data/example.md` → `problem_id=example`
- `data/algebra/modrep.md` → `problem_id=algebra/modrep`

**参考文献**：对于 `data/algebra/modrep.md`，关联的参考文献目录为 `data/algebra/modrep.refs/`。支持 `.md`、`.tex` 和 `.txt` 格式。PDF 文件由运行脚本预先提取为 `.txt`。

---

### 2. 自适应控制循环（核心工作方式）

智能体不是按固定步骤执行，而是在每次迭代中评估当前状态，动态选择最合适的技能：

| 技能 | 作用 |
|------|------|
| `search-math-results` | 搜索 arXiv 相关定理 |
| `query-memory` | 检索已保存的推理记录 |
| `construct-toy-examples` | 构造小规模例子探索模式 |
| `construct-counterexamples` | 构造反例检验猜想 |
| `propose-subgoal-decomposition-plans` | 将大问题分解为子目标 |
| `direct-proving` | 直接推理证明 |
| `recursive-proving` | 递归处理子目标 |
| `identify-key-failures` | 分析失败原因 |
| `verify-proof` | 调用验证服务 |
| `obtain-immediate-conclusions` | 从已知条件直接推导 |

每次迭代的决策逻辑：

1. **评估当前状态**：思考当前面临的主要问题、是否已充分搜索、能否提出多种分解方案等
2. **选择技能**：从上述 10 项技能中挑选最合适的
3. **执行并持久化**：将结果写入记忆通道

---

### 3. MCP 工具（Generation Agent ）

**位置**：[`mcp/server.py`](agents/generation/mcp/server.py)

提供 6 个工具供 Codex 智能体调用：

| 工具 | 功能 |
|------|------|
| `search_arxiv_theorems` | 通过 `leansearch.net/thm/search` 搜索相关定理、引理和定义 |
| `verify_proof_service` | 将当前陈述和证明 POST 到验证服务的 `http://127.0.0.1:8091/verify` |
| `memory_init` | 初始化持久化记忆目录，创建所有通道文件 |
| `memory_append` | 向指定通道追加 JSON 记录 |
| `memory_search` | 使用 BM25 算法搜索历史推理记录 |
| `branch_update` | 记录证明分支状态，支持回溯 |

---

### 4. 记忆系统

10 个独立通道（JSONL 文件），持久化在 `memory/{problem_id}/`：

| 通道 | 用途 |
|------|------|
| `immediate_conclusions` | 从已知条件直接推导的结论 |
| `toy_examples` | 构造的小规模例子 |
| `counterexamples` | 发现的反例 |
| `big_decisions` | 重大策略决策 |
| `subgoals` | 问题分解得到的子目标 |
| `proof_steps` | 证明步骤的记录 |
| `failed_paths` | 探索失败的路径 |
| `verification_reports` | 验证服务返回的报告 |
| `branch_states` | 分支状态（支持回溯） |
| `events` | 所有操作的事件日志 |

**BM25 检索**：`memory_search` 工具使用经典 BM25 算法（k1=1.5, b=0.75）对记忆通道进行文本搜索，按相关性排序返回结果。

---

### 5. 验证智能体（Verification Agent）

**位置**：[`agents/verification/`](agents/verification/)

#### 三层结构

**API 层**（[`api/server.py`](agents/verification/api/server.py)） — FastAPI 服务运行在 `localhost:8091`：

- `GET /health` — 健康检查
- `POST /verify` — 接收 `{statement, proof}`，生成 Run ID（格式：`{UTC时间戳}_{SHA-256前12位}`），调用 `codex exec` 执行验证智能体，将完整日志写入 `results/{run_id}/log.md`，返回 JSON 判决

**MCP 层**（[`mcp/server.py`](agents/verification/mcp/server.py)） — 提供 6 个工具：

- `search_arxiv_theorems` — 定理搜索
- `memory_init` / `memory_append` / `memory_query` — 记忆管理
- `validate_verification_output` — JSON Schema 验证
- `write_verification_output` — 写入 `results/{run_id}/verification.json`

**Schema**（[`schemas/verification_output.schema.json`](agents/verification/schemas/verification_output.schema.json)） — JSON Schema (Draft 2020-12)，定义了判决的严格输出格式，包含 `verification_report`、`verdict`、`repair_hints`。

#### 验证流程

1. 接收 `Run_id`、`Statement`、`Proof` 作为输入
2. **按顺序**逐条检查 proof 中的每个声明/子证明：
   - 检查逻辑推理的有效性
   - 检查定理是否正确应用
   - 检查是否缺少假设
   - 检查不合理的跳跃/模糊推理
3. 对外部引用通过 `search_arxiv_theorems` 先行验证，再辅以网络搜索
4. 构建验证报告：
   - `critical_errors`：严重错误（每条含 `location` 和 `issue`）
   - `gaps`：逻辑缺口（每条含 `location` 和 `issue`）
5. 输出判决：
   - `"correct"` — 零错误、零缺口
   - `"wrong"` — 附带修复提示 `repair_hints`

---

## 证明-修复循环

这是系统的**核心反馈机制**：

```
生成智能体写出证明（blueprint.md）
                │
                ▼
    调用 verify_proof_service
    POST /verify {statement, proof}
                │
                ▼
    验证智能体返回 JSON 判决
                │
        ┌───────┴───────┐
        ▼               ▼
    correct           wrong
        │               │
        ▼               ▼
    输出               读取 repair_hints
    blueprint_         修改证明
    verified.md        重新验证（loop）
                        │
                        └──────▶ 回到 verify_proof_service
```

生成智能体只有在验证通过后才会停止，否则会持续根据验证报告的修复提示进行修改。**14 条硬性约束**（定义在 `AGENTS.md` 中）确保智能体不会遗漏关键步骤：
1. 所有中间推理必须持久化
2. 必须使用外部引用并标注完整来源标识符
3. 在引用前必须检查上下文
4. 必须处理证明中的复杂部分，不可回避
5. ...（等 14 条规则）

---

## 完整工作流程

### 1. 环境准备

```bash
# 安装 Python 依赖
pip install -r agents/verification/requirements.txt
pip install -r agents/generation/mcp/requirements.txt

# 安装 Codex CLI
npm install -g @openai/codex

# 安装 Zola（可选，用于浏览结果）
brew install zola
```

### 2. 启动验证服务

```bash
cd agents/verification
uvicorn api.server:app --port 8091
```

验证服务在后台持续运行，等待生成智能体的验证请求。

### 3. 提交数学问题

在 `agents/generation/data/{category}/problem.md` 写入问题，可选的参考文献放在同名 `.refs/` 目录下。

### 4. 运行生成智能体

```bash
cd agents/generation
PROBLEM_FILE=data/example.md bash tests/run_example.sh
```

**`run_example.sh` 脚本执行过程**：
1. 验证 `PROBLEM_FILE` 路径合法性
2. 计算 `problem_id`
3. 检测并预提取 PDF 参考文献（使用 `pdftotext`）
4. 构造 Codex CLI 命令（含模型、推理强度配置）
5. 启动 Codex CLI，加载 AGENTS.md 和 MCP 工具
6. 实时显示运行时间（每 30 秒）
7. 检查验证服务是否可达（否则发出警告）
8. 将完整运行日志写入 `logs/{problem_id}/`

### 5. 智能体内部执行过程

```
读取问题文件
    │
    ▼
初始化记忆 (memory_init)
    │
    ▼
┌─────────────────────────────────┐
│      自适应控制循环               │
│                                 │
│  评估状态 → 选择技能 → 执行 → 持久化 │◀── 循环
│                                 │
│  search_arxiv_theorems          │
│  construct_toy_examples         │
│  propose_subgoal_decomposition  │
│  direct_proving                 │
│  verify_proof_service ──────────┼──→ 调用验证 API
│  ...                            │
└─────────────────────────────────┘
    │
    ▼
验证通过 → 输出 blueprint_verified.md
```

### 6. 产出结果

| 产物 | 路径 | 说明 |
|------|------|------|
| 证明草稿 | `results/{problem_id}/blueprint.md` | 工作过程中的证明 |
| 验证通过证明 | `results/{problem_id}/blueprint_verified.md` | 最终验证通过的证明 |
| 推理记忆 | `memory/{problem_id}/` | 10 个通道的完整 JSONL 文件 |
| 运行日志 | `logs/{problem_id}/` | Codex CLI 完整输出 |
| 验证判决 | `../verification/results/{run_id}/verification.json` | 结构化验证结果 |

### 7. 浏览结果（可选）

```bash
cd agents/generation
bash site/serve.sh
```

启动 Zola 静态站点（端口 3264），用浏览器查看所有已完成的证明。站点使用：

- **MATbook 主题**（类似教材风格）
- **MathJax 3** 渲染数学公式
- **深色模式**切换
- **TiKZ** 图形支持
- `transform_math.py` 预处理 LaTeX 分隔符以兼容 Zola + MathJax

---

## 已完成的运行示例

项目中包含一个完整的端到端运行示例：

- **问题**：`data/example.md` — "Prove that every finite group of prime order is cyclic."（证明每个素数阶有限群都是循环群）
- **生成结果**：[`results/example/`](agents/generation/results/example/) 中的 `blueprint.md` 和 `blueprint_verified.md`
- **记忆**：11 个 JSONL 文件，记录了完整的推理过程
- **验证判决**：[`verification/results/20260530T155155Z_67615df2ec7e/verification.json`](agents/verification/results/20260530T155155Z_67615df2ec7e/verification.json) — `"verdict": "correct"`，零错误零缺口

---

## 关键设计理念

| 理念 | 说明 |
|------|------|
| **指令驱动** | 智能体的行为完全由 `AGENTS.md` 用自然语言定义，没有硬编码的控制流，Codex CLI 负责理解和执行这些指令 |
| **持久化记忆** | 所有中间推理都持久化为 JSONL 文件，支持长程推理和上下文恢复 |
| **BM25 检索** | 记忆搜索使用经典信息检索算法（BM25），按相关性排序历史推理，使智能体能有效回溯已探索的内容 |
| **模块化验证** | 验证智能体是独立的 HTTP 服务，可以被任意生成智能体复用，解耦证明生成和正确性检查 |
| **严格约束** | 14 条硬性规则作为不可变约束，确保智能体不会跳过关键步骤（如必须持久化、必须外部引用完整来源等） |
| **自适应控制** | 智能体根据当前状态动态选择技能，而非固定流水线 —— 对简单问题快速完成，对困难问题深入探索 |
| **证明-修复循环** | 验证不通过时，智能体根据修复提示修改证明并重新验证，直到通过或所有路径耗尽 |

---

## 环境变量

| 变量 | 默认值 | 说明 |
|------|--------|------|
| `MODEL` | `gpt-5.4` | Codex CLI 使用的模型 |
| `REASONING_EFFORT` | `xhigh` | 推理强度（生成智能体） |
| `CODEX_BIN` | `codex` | Codex CLI 可执行文件路径 |
| `CODEX_MODEL` | `gpt-5.4` | 验证智能体使用的模型 |
| `CODEX_REASONING_EFFORT` | `xhigh` | 验证智能体推理强度 |
| `CODEX_TIMEOUT_SECONDS` | 无限制 | 验证超时时间 |
