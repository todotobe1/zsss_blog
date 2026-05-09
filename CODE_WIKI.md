# Trae Agent 项目 Code Wiki

## 项目概述

### 项目简介

Trae Agent 是由字节跳动（ByteDance）开发的基于大语言模型（LLM）的通用软件工程任务代理。该项目提供了一个强大的命令行界面（CLI），能够理解自然语言指令，并通过各种工具和 LLM 提供商执行复杂的软件工程工作流程。

### 核心特性

| 特性 | 描述 |
|------|------|
| Lakeview | 为代理步骤提供简洁的摘要功能 |
| 多 LLM 支持 | 支持 OpenAI、Anthropic、Doubao、Azure、OpenRouter、Ollama 和 Google Gemini 等多种 API |
| 丰富工具生态 | 提供文件编辑、Bash 执行、结构化思考等工具 |
| 交互模式 | 提供对话式界面，支持迭代开发 |
| 轨迹记录 | 详细记录所有代理操作，便于调试和分析 |
| 灵活配置 | 基于 YAML 的配置系统，支持环境变量 |
| Docker 模式 | 支持在 Docker 容器中运行任务 |
| 易于安装 | 基于 pip 的简单安装方式 |

### 技术报告

该项目基于学术研究，发表了技术报告《Trae Agent: An LLM-based Agent for Software Engineering with Test-time Scaling》，可在 arXiv (arXiv:2507.23370) 获取详细技术信息。

---

## 项目架构总览

### 整体架构设计

Trae Agent 采用模块化设计，遵循清晰的分层架构原则。整个系统由以下几个核心层次构成：

**表现层（Presentation Layer）**：负责与用户的交互，主要包括 CLI 命令行界面和交互式控制台。该层处理用户输入的命令和参数，并将执行结果以友好的方式展示给用户。

**代理层（Agent Layer）**：核心的业务逻辑层，包含 Agent 类的实现。这一层负责协调整个任务的执行流程，包括与 LLM 的交互、工具的调度、状态的维护等。

**工具层（Tool Layer）**：提供各种可执行的工具，供代理层调用。包括文件编辑工具、Bash 执行工具、结构化思考工具等。每种工具都有标准的接口定义，便于扩展和集成。

**通信层（Communication Layer）**：负责与各种 LLM 提供商的通信。封装了不同 API 的调用方式，提供统一的接口给代理层使用。

**配置层（Configuration Layer）**：管理系统配置，包括 YAML 配置文件解析、环境变量处理、命令行参数优先级等。

**评估层（Evaluation Layer）**：提供评估框架，用于测试和验证代理的性能。

---

## 目录结构分析

### 顶层目录结构

```
trae-agent/
├── .github/              # GitHub 配置文件（Issue 模板、PR 模板、Workflows）
├── .vscode/              # VSCode 配置（调试模板）
├── docs/                 # 项目文档
├── evaluation/          # 评估框架
├── server/               # HTTP 服务器实现
├── tests/                # 测试代码
├── trae_agent/           # 核心代码包
├── .gitignore            # Git 忽略配置
├── .pre-commit-config.yaml  # Pre-commit 配置
├── .python-version       # Python 版本指定
├── CONTRIBUTING.md       # 贡献指南
├── LICENSE               # MIT 许可证
├── Makefile              # 构建和任务自动化
├── README.md             # 项目说明
├── pyproject.toml        # 项目配置和依赖定义
├── trae_config.json.example  # JSON 配置示例
└── trae_config.yaml.example # YAML 配置示例
```

### 核心代码包结构（trae_agent/）

trae_agent/ 目录包含项目的主要实现代码，其结构如下：

```
trae_agent/
├── __init__.py              # 包初始化，导出核心类和接口
├── cli.py                   # 命令行接口主入口
├── agent/                   # 代理核心实现
│   ├── __init__.py
│   ├── base_agent.py        # 基础代理类
│   ├── trae_agent.py        # Trae Agent 具体实现
│   └── ...
├── tools/                   # 工具实现
│   ├── __init__.py
│   ├── base.py              # 工具基类和执行器
│   ├── str_replace_based_edit_tool.py  # 文件编辑工具
│   ├── bash.py              # Bash 执行工具
│   ├── sequential_thinking.py  # 结构化思考工具
│   ├── task_done.py         # 任务完成工具
│   ├── json_edit_tool.py    # JSON 编辑工具
│   └── ...
├── utils/                   # 工具函数
│   ├── __init__.py
│   ├── config.py            # 配置管理
│   ├── cli.py               # CLI 控制台
│   ├── llm_clients/         # LLM 客户端
│   │   ├── __init__.py
│   │   ├── llm_client.py    # 基础 LLM 客户端
│   │   ├── openai_client.py
│   │   ├── anthropic_client.py
│   │   ├── google_client.py
│   │   ├── doubao_client.py
│   │   ├── openrouter_client.py
│   │   └── ollama_client.py
│   └── ...
├── docker/                  # Docker 支持
│   ├── __init__.py
│   ├── docker_manager.py    # Docker 管理器
│   └── ...
└── dist/                    # PyInstaller 打包的二进制文件
```

### 评估模块结构（evaluation/）

```
evaluation/
├── __init__.py
├── README.md
├── run_evaluation.py        # 评估主程序
├── setup.sh                 # 环境设置脚本
├── utils.py                 # 评估工具函数
├── patch_selection/         # 补丁选择模块
│   ├── README.md
│   ├── analysis.py          # 分析工具
│   ├── selector.py          # 选择器
│   ├── trae_selector/       # Trae 选择器实现
│   │   ├── sandbox.py       # 沙箱环境
│   │   ├── selector_agent.py
│   │   ├── selector_evaluation.py
│   │   └── utils.py
│   └── example/             # 示例数据
└── ...
```

---

## 主要模块职责

### 命令行接口模块（cli.py）

cli.py 是整个应用程序的入口点，定义了命令行界面的所有命令和选项。该模块主要完成以下工作：

**命令定义**：使用 Click 框架定义了三个主要命令——run（运行任务）、interactive（交互模式）和 show-config（显示配置）。

**参数处理**：处理各种命令行参数，包括任务描述、提供商选择、模型选择、工作目录、最大步数、Docker 配置等。支持从配置文件、环境变量和命令行参数多层次获取配置。

**Docker 支持**：提供完整的 Docker 模式支持，包括使用指定镜像运行、在新容器中执行、附加到已有容器、从 Dockerfile 构建、从本地镜像文件加载等功能。

**配置解析**：实现了配置文件的后向兼容性，自动检测 YAML 和 JSON 配置文件格式，并按照命令行参数 > 配置文件 > 环境变量 > 默认值的优先级解析配置。

### 代理核心模块（agent/）

代理模块是 Trae Agent 的核心，实现了任务执行的完整逻辑。

**BaseAgent 类**：作为所有代理类型的基类，定义了代理的基本接口和通用功能。负责维护代理的状态、管理消息历史、处理工具调用等。

**TraeAgent 类**：Trae Agent 的具体实现，继承自 BaseAgent。该类包含以下核心功能：

任务执行循环：不断接收用户指令，与 LLM 交互，执行工具调用，直到任务完成或达到最大步数限制。

消息管理：维护完整的对话历史，包括系统消息、用户消息、助手消息和工具调用结果。

工具调度：根据 LLM 的响应选择和调用相应的工具，并处理工具的执行结果。

轨迹记录：将整个执行过程记录到 JSON 文件中，便于后续分析和调试。

### 工具模块（tools/）

工具模块提供了代理执行任务所需的各种能力，每种工具都遵循统一的接口规范。

**Tool 基类**：定义了所有工具必须实现的标准接口，包括工具名称、描述、参数模式等。每个工具都需要提供 schema 方法来描述其输入参数，以及 execute 方法来执行具体操作。

**str_replace_based_edit_tool**：功能强大的文件编辑工具，支持多种文件操作。view 操作用于查看文件内容或列出目录结构，支持指定行范围查看大文件。create 操作用于创建新文件，如果文件已存在则失败。str_replace 操作用于精确字符串替换，必须提供唯一匹配的字符串。insert 操作用于在指定行号后插入文本。所有路径必须使用绝对路径。

**bash 工具**：提供在持久化 Bash 会话中执行 shell 命令的能力。命令在共享的 Bash 会话中执行，能够保持状态。每个命令有 120 秒的超时限制，支持会话重启和后台进程管理。

**sequential_thinking 工具**：结构化问题解决工具，帮助代理进行复杂问题的分析和推理。支持将问题分解为连续的思考步骤，允许修订和分支，能够动态调整思考深度。工具会跟踪思考历史和替代方案。

**task_done 工具**：任务完成信号工具，用于标记任务成功完成。代理在调用此工具前必须进行适当验证，通常需要编写测试或复现脚本来确认任务正确完成。

**json_edit_tool**：精确的 JSON 文件编辑工具，使用 JSONPath 表达式定位和修改内容。支持 view（查看）、set（更新）、add（添加）、remove（删除）等操作，提供详细的错误信息。

### 工具执行器（ToolExecutor）

ToolExecutor 是工具执行的管理器，负责工具的注册、调度和结果处理。它维护一个工具注册表，根据工具名称路由调用请求，并处理工具执行过程中的异常情况。

### LLM 客户端模块（utils/llm_clients/）

LLM 客户端模块封装了与各种 LLM 提供商的通信逻辑，提供统一的接口给代理层使用。

**LLMClient 基类**：定义了 LLM 客户端的通用接口，包括聊天完成、工具调用等功能。每个具体的客户端实现都需要继承这个基类。

**OpenAI 客户端**：支持 OpenAI 的 GPT 系列模型，实现完整的 API 调用逻辑。

**Anthropic 客户端**：支持 Anthropic 的 Claude 系列模型，包括 Claude 3.5 Sonnet 等。

**Google 客户端**：支持 Google Gemini 系列模型的调用。

**Doubao 客户端**：支持字节跳动的豆包模型。

**OpenRouter 客户端**：支持 OpenRouter 聚合服务，可以访问多种模型。

**Ollama 客户端**：支持本地部署的 Ollama 模型服务。

### 配置管理模块（utils/config.py）

配置管理模块负责整个系统的配置处理，支持多种配置来源和格式。

**Config 类**：主配置类，负责从 YAML 或 JSON 文件加载配置，并支持通过命令行参数和环境变量覆盖。

**TraeAgentConfig 类**：Trae Agent 的具体配置，包括模型选择、工具配置、最大步数等。

**配置优先级**：命令行参数 > 配置文件 > 环境变量 > 默认值。

---

## 关键类与函数详解

### 命令行入口函数

**main 函数**：位于 trae_agent.cli 模块，是整个应用的入口点。通过 setuptools 的 entry_points 配置，在安装后可通过 trae-cli 命令调用。

```python
trae-cli run "执行的任务描述" --provider openai --model gpt-4o
```

### CLI 命令详解

**run 命令**：执行单次任务的命令，是 Trae Agent 最常用的使用方式。

| 参数 | 类型 | 说明 |
|------|------|------|
| task | 字符串 | 要执行的任务描述（可选，与 --file 互斥） |
| --file, -f | 路径 | 包含任务描述的文件路径 |
| --provider, -p | 字符串 | LLM 提供商（openai/anthropic/google 等） |
| --model, -m | 字符串 | 具体使用的模型名称 |
| --working-dir, -w | 路径 | 工作目录，代理的操作基于此目录 |
| --max-steps | 整数 | 最大执行步数限制 |
| --must-patch | 标志 | 是否强制生成补丁 |
| --trajectory-file, -t | 路径 | 轨迹记录文件保存路径 |
| --docker-image | 字符串 | Docker 镜像名称 |
| --docker-container-id | 字符串 | 已有 Docker 容器 ID |
| --config-file | 路径 | 配置文件路径（默认 trae_config.yaml） |

**interactive 命令**：启动交互式会话，允许用户多次提交任务。

```python
trae-cli interactive --provider anthropic --model claude-sonnet-4-20250514
```

### 代理核心类

**Agent 类**（在 agent/__init__.py 中导入）：代理工厂类，根据 agent_type 创建对应的代理实例。

```python
agent = Agent(
    agent_type="trae_agent",
    config=config,
    trajectory_file="output.json",
    cli_console=console,
    docker_config=docker_config,
    docker_keep=True
)
```

**BaseAgent 类**：基础代理抽象类，定义代理的通用接口。

主要方法包括：run() 异步方法，执行完整任务；step() 单步执行方法，处理一个思考-动作循环；think() 与 LLM 交互获取响应；execute_tool() 执行具体工具；save_trajectory() 保存执行轨迹。

**TraeAgent 类**：Trae Agent 的具体实现，继承自 BaseAgent。包含更丰富的任务执行逻辑，集成了 Lakeview 摘要功能。

### 工具相关类

**Tool 抽象基类**：所有工具的基类。

```python
class Tool(ABC):
    @property
    @abstractmethod
    def name(self) -> str: pass

    @property
    @abstractmethod
    def description(self) -> str: pass

    @abstractmethod
    def schema(self) -> dict: pass

    @abstractmethod
    async def execute(self, **kwargs) -> str: pass
```

**ToolExecutor 类**：工具执行管理器。

```python
class ToolExecutor:
    def __init__(self, tools: list[Tool]):
        self._tools = {tool.name: tool for tool in tools}

    async def execute(self, tool_name: str, **kwargs) -> str:
        if tool_name not in self._tools:
            raise ValueError(f"Unknown tool: {tool_name}")
        return await self._tools[tool_name].execute(**kwargs)
```

### 配置相关类

**Config 类**：主配置管理类。

```python
config = Config.create(config_file="trae_config.yaml")
config = config.resolve_config_values(
    provider="openai",
    model="gpt-4o",
    max_steps=100
)
```

**Config.create 工厂方法**：根据文件扩展名自动选择解析器（YAML 或 JSON）。

### LLM 客户端接口

**LLMClient 抽象类**：所有 LLM 客户端的基类。

```python
class LLMClient(ABC):
    @abstractmethod
    async def chat_complete(
        self,
        messages: list[dict],
        tools: list[dict] | None = None,
        **kwargs
    ) -> ChatCompletionResponse: pass
```

---

## 依赖关系

### 项目依赖概述

Trae Agent 基于 Python 3.12+ 开发，使用 pyproject.toml 管理项目依赖。所有核心依赖分为以下几个类别：

### 核心运行时依赖

**openai (>=1.86.0)**：OpenAI API 客户端库，用于与 OpenAI 模型通信。

**anthropic (>=0.54.0, <=0.60.0)**：Anthropic API 客户端，用于 Claude 系列模型的调用。

**google-genai (>=1.24.0)**：Google Generative AI 库，支持 Gemini 模型。

**ollama (>=0.5.1)**：Ollama 本地模型服务客户端。

### CLI 和界面依赖

**click (>=8.0.0)** 和 **asyncclick (>=8.0.0)**：命令行界面框架。

**rich (>=13.0.0)**：富文本和美化输出的库。

**textual (>=0.50.0)**：终端用户界面框架。

**pyyaml (>=6.0.2)**：YAML 配置文件解析。

### 数据处理依赖

**pydantic (>=2.0.0)**：数据验证和设置管理。

**python-dotenv (>=1.0.0)**：环境变量管理，支持 .env 文件。

**jsonpath-ng (>=1.7.0)**：JSONPath 表达式解析，用于 JSON 编辑工具。

### 代码分析依赖

**tree-sitter (==0.21.3)** 和 **tree-sitter-languages (==1.10.2)**：代码解析库，支持多语言的语法分析。

### 代理协议依赖

**mcp (==1.12.2)**：Model Context Protocol，模型上下文协议支持。

### 开发工具依赖

**ruff (>=0.12.4)**：Python 代码检查和格式化工具。

**pyinstaller (==6.15.0)**：Python 程序打包工具，用于 Docker 模式。

### 可选测试依赖

**pytest (>=8.0.0)**：单元测试框架。

**pytest-asyncio (>=0.23.0)**：异步测试支持。

**pytest-mock (>=3.12.0)**：测试模拟工具。

**pytest-cov (>=4.0.0)**：代码覆盖率工具。

**pre-commit (>=4.2.0)**：Git pre-commit 钩子管理。

### 可选评估依赖

**datasets (>=3.6.0)**：数据集处理库。

**docker (>=7.1.0)**：Docker Python SDK。

**pexpect (>=4.9.0)**：伪终端控制库。

**unidiff (>=0.7.5)**：统一 diff 解析库。

### 依赖关系图

```
trae_agent
├── cli.py (Click, Rich, Textual)
├── agent/
│   ├── BaseAgent (LLMClient, ToolExecutor)
│   └── TraeAgent
├── tools/
│   ├── base.py (Tool, ToolExecutor)
│   ├── str_replace_based_edit_tool.py
│   ├── bash.py
│   ├── sequential_thinking.py
│   ├── task_done.py
│   └── json_edit_tool.py (jsonpath-ng)
└── utils/
    ├── config.py (PyYAML, python-dotenv, Pydantic)
    ├── cli.py (Rich, Textual)
    └── llm_clients/
        ├── llm_client.py (ABC)
        ├── openai_client.py (OpenAI)
        ├── anthropic_client.py (Anthropic)
        ├── google_client.py (Google GenAI)
        ├── doubao_client.py
        ├── openrouter_client.py
        └── ollama_client.py (Ollama)

evaluation/
├── run_evaluation.py (Docker, datasets, pexpect)
└── patch_selection/
    └── trae_selector/
        ├── selector_agent.py
        └── sandbox.py (Docker)
```

---

## 项目运行方式

### 环境准备

#### 系统要求

- Python 3.12 或更高版本
- UV 包管理器（推荐）或 pip
- Docker（可选，用于 Docker 模式）
- 至少 8GB 内存
- 稳定的网络连接（用于 API 调用）

#### 安装步骤

**方式一：开发安装（推荐贡献者）**

```bash
# 克隆仓库
git clone https://github.com/bytedance/trae-agent.git
cd trae-agent

# 创建虚拟环境并安装所有依赖
make install-dev

# 安装 pre-commit 钩子
make pre-commit-install
```

**方式二：标准安装**

```bash
# 使用 UV
uv sync --all-extras
source .venv/bin/activate

# 或使用 pip
pip install -e ".[test,evaluation]"
```

### 配置文件

#### YAML 配置（推荐）

```yaml
agents:
  trae_agent:
    enable_lakeview: true
    model: trae_agent_model
    max_steps: 200
    tools:
      - bash
      - str_replace_based_edit_tool
      - sequentialthinking
      - task_done

model_providers:
  anthropic:
    api_key: your_anthropic_api_key
    provider: anthropic
  openai:
    api_key: your_openai_api_key
    provider: openai

models:
  trae_agent_model:
    model_provider: anthropic
    model: claude-sonnet-4-20250514
    max_tokens: 4096
    temperature: 0.5

mcp_servers:
  playwright:
    command: npx
    args:
      - "@playwright/mcp@0.0.27"
```

#### 环境变量配置

```bash
export OPENAI_API_KEY="your-openai-api-key"
export ANTHROPIC_API_KEY="your-anthropic-api-key"
export GOOGLE_API_KEY="your-google-api-key"
```

### 常用命令

#### 单任务执行

```bash
# 基本用法
trae-cli run "创建一个人脸识别的 Python 脚本"

# 指定模型和提供商
trae-cli run "修复 main.py 中的认证 bug" --provider openai --model gpt-4o

# 自定义工作目录
trae-cli run "为 utils 模块添加测试" --working-dir /path/to/project

# 保存轨迹记录
trae-cli run "优化数据库查询" --trajectory-file debug_session.json

# 强制生成补丁
trae-cli run "更新 API 端点" --must-patch
```

#### 交互模式

```bash
# 启动交互式会话
trae-cli interactive

# 自定义配置启动
trae-cli interactive --provider anthropic --model claude-sonnet-4-20250514 --max-steps 30
```

#### Docker 模式

```bash
# 使用指定镜像
trae-cli run "添加测试" --docker-image python:3.11

# 挂载工作目录
trae-cli run "编写 hello world 脚本" --docker-image python:3.12 --working-dir test_workdir/

# 附加到已有容器
trae-cli run "更新 API" --docker-container-id 91998a56056c

# 从 Dockerfile 构建
trae-cli run "调试认证" --dockerfile-path test_workspace/Dockerfile

# 使用本地镜像
trae-cli run "修复 bug" --docker-image-file test_workspace/trae_agent_custom.tar
```

### 测试运行

```bash
# 运行所有测试（跳过外部服务测试）
make test

# 使用 UV 运行测试
make uv-test

# 运行特定测试
pytest tests/ -v

# 生成覆盖率报告
pytest tests/ --cov=trae_agent --cov-report=html
```

### 代码质量检查

```bash
# 安装并运行 pre-commit
make pre-commit

# 单独运行 pre-commit
make pre-commit-run

# 自动修复格式问题
make fix-format

# 运行 ruff 检查
ruff check .
ruff format .
```

---

## 扩展与定制

### 添加新工具

要添加新的工具，需要继承 Tool 基类并实现必要的方法：

```python
from trae_agent.tools.base import Tool
from typing import Any

class MyCustomTool(Tool):
    @property
    def name(self) -> str:
        return "my_custom_tool"

    @property
    def description(self) -> str:
        return "执行自定义操作的工具"

    def schema(self) -> dict:
        return {
            "name": "my_custom_tool",
            "description": self.description,
            "parameters": {
                "type": "object",
                "properties": {
                    "param1": {"type": "string", "description": "参数1"}
                },
                "required": ["param1"]
            }
        }

    async def execute(self, **kwargs) -> str:
        param1 = kwargs.get("param1")
        # 执行逻辑
        return f"结果: {param1}"
```

### 添加新 LLM 提供商

要支持新的 LLM 提供商，需要实现 LLMClient 的子类：

```python
from abc import ABC
from trae_agent.utils.llm_clients.llm_client import LLMClient, ChatCompletionResponse

class MyProviderClient(LLMClient):
    def __init__(self, api_key: str, base_url: str | None = None, **kwargs):
        super().__init__(api_key, base_url, **kwargs)
        # 初始化提供商特定的客户端

    async def chat_complete(
        self,
        messages: list[dict],
        tools: list[dict] | None = None,
        **kwargs
    ) -> ChatCompletionResponse:
        # 实现与提供商的 API 通信
        pass
```

### 配置 MCP 服务器

MCP (Model Context Protocol) 服务器可以扩展代理的工具能力：

```yaml
mcp_servers:
  playwright:
    command: npx
    args:
      - "@playwright/mcp@0.0.27"
  filesystem:
    command: "npx"
    args:
      - "-y"
      - "@modelcontextprotocol/server-filesystem"
      - "/path/to/allowed/directory"
```

---

## 开发指南

### 项目结构规范

- 核心代码放在 trae_agent/ 目录下
- 测试代码放在 tests/ 目录下
- 评估工具放在 evaluation/ 目录下
- HTTP 服务器代码放在 server/ 目录下

### 代码风格

项目使用 Ruff 进行代码检查和格式化：

- 行长度限制：100 字符
- 启用的规则集：B, SIM, C4, E4, E9, E7, F, I
- 使用类型注解
- 所有公开类和方法需要文档字符串

### 提交规范

```bash
# 创建功能分支
git checkout -b feature/amazing-feature

# 提交代码（遵循conventional commits）
git commit -m 'feat: add amazing feature'
git commit -m 'fix: resolve authentication bug'
git commit -m 'docs: update README'

# 推送并创建 PR
git push origin feature/amazing-feature
```

### Pull Request 检查清单

- [ ] 所有测试通过
- [ ] 没有 linting 错误
- [ ] 添加了新功能的测试
- [ ] 更新了相关文档
- [ ] PR 描述完整清楚
- [ ] 关联了相关 issue

---

## 常见问题

### 导入错误

```bash
# 设置 PYTHONPATH
PYTHONPATH=. trae-cli run "your task"
```

### API Key 问题

```bash
# 检查环境变量
echo $OPENAI_API_KEY

# 验证配置
trae-cli show-config
```

### 命令找不到

```bash
# 使用 uv 运行
uv run trae-cli run "your task"

# 或激活虚拟环境
source .venv/bin/activate
trae-cli run "your task"
```

### Docker 权限问题

```bash
# 确保用户属于 docker 组
sudo usermod -aG docker $USER
newgrp docker
```

---

## 参考资源

### 官方文档

- 项目主页：https://github.com/bytedance/trae-agent
- 技术报告：https://arxiv.org/abs/2507.23370
- 官方文档：https://www.trae.ai/

### 相关项目

- Anthropic Quickstart：https://github.com/anthropics/anthropic-quickstarts
- Tree-sitter：用于代码解析
- Model Context Protocol (MCP)：用于扩展工具能力

### 社区支持

- GitHub Issues：报告 bug 和请求功能
- GitHub Discussions：社区讨论
- Discord：实时交流（https://discord.gg/VwaQ4ZBHvC）

---

## 许可证

本项目采用 MIT 许可证开源。详细许可证内容请参考项目根目录的 LICENSE 文件。

---

*本文档由 Code Wiki 生成工具自动创建，最后更新于 2026 年 2 月*
