# AI Agents with LangChain

A personal learning workspace for building AI agents with [LangChain](https://python.langchain.com/), [LangGraph](https://langchain-ai.github.io/langgraph/) and [Deep Agents](https://academy.langchain.com/courses/foundation-introduction-to-deepagents). It contains course notebooks and a separate Deep Agents project.

## Repository layout

```text
.
├── SimpleAgent/                    # Intro notebooks (root project)
│   ├── 1.1_foundational_models.ipynb   # init_chat_model, providers, create_agent, streaming
│   └── 1.1_prompting.ipynb             # system prompts, few-shot, structured output
├── DeepAgents/                     # Self-contained Deep Agents course project
│   ├── models.py                       # Central model config (default: Anthropic Claude Haiku)
│   ├── pyproject.toml                  # Its own dependencies (deepagents==0.7.0)
│   ├── .env.example                    # Template for its API keys
│   └── README.md                       # Course-specific setup and troubleshooting
├── src/ai_agents_langchain/        # Minimal package stub (entry point: ai-agents-langchain)
├── env_utils.py                    # Environment checker (Python, venv, API keys, packages)
├── pyproject.toml / uv.lock        # Root project dependencies
└── requirements.txt                # pip-compatible dependency list
```

There are **two separate Python projects**: the root one (for `SimpleAgent/`) and `DeepAgents/`. Each has its own `pyproject.toml`, `.env` and virtual environment.

## Prerequisites

- Python 3.12+ (root project; `DeepAgents/` supports 3.11–3.14)
- [uv](https://docs.astral.sh/uv/getting-started/installation/) (recommended) or pip
- At least one model provider API key (OpenAI, Anthropic or Google)
- Optional: [LangSmith](https://smith.langchain.com/) key for tracing, [Tavily](https://app.tavily.com) key for web-search examples

## Quick start (root project)

```bash
# Install dependencies into .venv
uv sync

# or with pip
python -m venv .venv
.venv\Scripts\activate          # macOS/Linux: source .venv/bin/activate
pip install -r requirements.txt
```

Create a `.env` file in the repo root with the keys you need (never commit it):

```bash
OPENAI_API_KEY=...        # the SimpleAgent notebooks use gpt-5-nano by default
ANTHROPIC_API_KEY=...     # optional
GOOGLE_API_KEY=...        # optional (Gemini)
LANGSMITH_API_KEY=...     # optional, for tracing
LANGSMITH_TRACING=true
TAVILY_API_KEY=...        # optional
```

Verify your setup:

```bash
uv run python env_utils.py
```

Then launch Jupyter and open a notebook in `SimpleAgent/`:

```bash
uv run jupyter lab
```

## Deep Agents project

```bash
cd DeepAgents
cp .env.example .env      # then fill in your keys
uv sync
uv run python env_utils.py
```

Model selection lives in [DeepAgents/models.py](DeepAgents/models.py). To switch providers (Azure, Bedrock, Google, Ollama, OpenRouter, ...), follow the instructions in that file. Some providers need an extra, e.g. `uv sync --extra google`. See [DeepAgents/README.md](DeepAgents/README.md) for full setup and troubleshooting.

## What's covered

| Notebook | Topics |
| --- | --- |
| `1.1_foundational_models` | Initialising chat models, customising parameters, switching providers (OpenAI, Anthropic, Gemini), creating agents with `create_agent`, multi-turn messages, token streaming |
| `1.1_prompting` | System prompts, few-shot examples, structured prompts, structured output with Pydantic |

## Key dependencies

`langchain`, `langgraph`, `langchain-openai`, `langchain-anthropic`, `langchain-google-genai`, `langchain-mcp-adapters`, `langsmith`, `langchain-tavily`, `jupyterlab`. See [pyproject.toml](pyproject.toml) for versions.

## Troubleshooting

- **`ModuleNotFoundError`**: you're probably outside the virtual environment. Use `uv run ...` or activate `.venv`.
- **Key not picked up**: `load_dotenv()` doesn't override variables already set in your OS environment. Remove the conflicting variable or use `load_dotenv(override=True)`. `env_utils.py` reports such conflicts.
- **Tracing errors**: set `LANGSMITH_TRACING=false` if you have no LangSmith key.

## Security

`.env` files hold secrets and are excluded via [.gitignore](.gitignore). Never commit API keys.
