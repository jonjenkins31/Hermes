# Hermes Agent - Local Model Testing Project

⚕️ **Hermes Agent** testbed with enhanced **local inference support** for offline AI agents.

This repository is a **testing and development environment** for the Python Hermes Agent, extended with multi-engine local model support including **llama.cpp**, **MLX**, and other inference runtimes for fully offline operation.

## 🎯 Purpose

This project enables:
- **Local-first AI agents** - Run Hermes Agent entirely offline with GGUF models
- **Multi-engine inference** - Support for llama.cpp, MLX, vLLM, and other runtimes
- **Model benchmarking** - Compare cloud vs local models systematically
- **Agentic robotics** - Framework for real-world robotics requiring local execution

## 🏗️ Architecture

```
Hermes/
├── python_hermes_agent/          # Core Hermes Agent + Local Extensions
│   ├── upstream/                 # Original Hermes Agent source
│   ├── runtime_framework/        # ⭐ Multi-engine runtime support
│   │   ├── runtimes/            # Engine binaries (llama.cpp, MLX, vLLM)
│   │   ├── scripts/             # Runtime management tools
│   │   └── docs/                # Framework documentation
│   ├── workspace/                # Instance-specific data
│   │   ├── config.yaml          # Agent configuration
│   │   ├── memories/            # Conversation history
│   │   └── logs/                # Runtime logs
│   ├── skills/                   # Agent capabilities
│   ├── benchmark/                # Model benchmarking tools
│   └── scripts/                  # Utility scripts
├── models/                       # Shared model storage (optional)
├── docs/                         # Project documentation
├── agent_doctor.py               # ⭐ Robot pre-flight health checks
├── supervisor.py                 # ⭐ Restart-on-crash supervisor
├── main.py                       # Multi-agent orchestrator
├── model_resolver.py             # Model path resolution
└── requirements.txt              # Python dependencies
```

## 🚀 Quick Start

### Prerequisites

```bash
# macOS with Homebrew
brew install python@3.11

# Clone the repository
cd ~/GitHub/GITHUB/Hermes

# Create virtual environment
python3 -m venv .venv
source .venv/bin/activate

# Install Hermes Agent
pip install -e python_hermes_agent/upstream/.

# Install local inference support
pip install llama-cpp-python
# For GPU acceleration on Mac:
# CMAKE_ARGS="-DGGML_METAL=on" pip install llama-cpp-python --force-reinstall
```

### Configure Local Models

```bash
# Set up your first local model
export LLAMA_CPP_MODEL_PATH=~/.lmstudio/models/lmstudio-community/Qwen3.5-9B-GGUF/Qwen3.5-9B-Q4_K_M.gguf

# Or use the model picker
hermes model local/lmstudio/Qwen3.5-9B-Q4_K_M

# Verify configuration
hermes config show
```

### Run the Agent

```bash
# Start a chat session
hermes chat "Hello, I'm testing local models!"

# Or use quiet mode for quick queries
hermes chat -q "What's 2+2?"
```

## 🦙 Supported Inference Engines

| Engine | Best For | Models | Status |
|--------|----------|--------|--------|
| **llama.cpp** | General GGUF models | Qwen, Gemma, Llama, Mistral | ✅ Production |
| **MLX** | Apple Silicon optimization | MLX-native models | ⚠️ Experimental |
| **vLLM** | High-throughput serving | HuggingFace models | 🔜 Coming soon |

### Engine Management

```bash
# List available engines
python python_hermes_agent/runtime_framework/scripts/runtime_manager.py list

# Switch engine version
python python_hermes_agent/runtime_framework/scripts/runtime_manager.py use llama.cpp 2.16.0

# Check model compatibility
python python_hermes_agent/runtime_framework/scripts/runtime_manager.py compat /path/to/model.gguf
```

## 🤖 Robotics Components

This project includes critical infrastructure for **autonomous robot operation**:

### `agent_doctor.py` - Pre-flight Health Checks

Run before deploying or letting the robot operate autonomously:

```bash
# Quick health check
.venv/bin/python agent_doctor.py

# Verbose output
.venv/bin/python agent_doctor.py --verbose
```

**Checks performed:**
- ✅ Python version compatibility (3.10-3.12)
- ✅ Model file existence and accessibility
- ✅ Memory integrity
- ✅ Disk space availability
- ✅ Required dependencies installed

**Designed for:**
- systemd units that gate robot startup on health check success
- Pre-deployment verification before autonomous operation
- CI/CD pipelines for robot software updates

### `supervisor.py` - Restart-on-Crash Supervisor

Keeps the robot agent running unattended with exponential backoff:

```bash
# Supervise a voice loop
python supervisor.py -- python -m python_jaeger.plugins.voice_loop

# Supervise with custom restart limit
python supervisor.py --max-restarts 50 -- python main.py python_pydantic_ai
```

**Features:**
- 🔄 Automatic restart on crash (segfaults, OOM, etc.)
- ⏱️ Exponential backoff (1s → 60s max)
- 📝 Crash logging with stderr capture
- 🎯 Backoff resets after "good runs" (>60s uptime)
- 🛑 Clean shutdown on Ctrl-C (forwards SIGTERM)

**Use cases:**
- Long-running robot assistants that must stay up 24/7
- Production deployments where manual restart isn't feasible
- Metal/llama-cpp segfault recovery (common with GPU acceleration)

### `main.py` - Multi-Agent Orchestrator

Coordinates multiple agent instances and frameworks:

```bash
# Run different agent frameworks
python main.py python_hermes_xml "search the web for..."
python main.py python_pydantic_ai "tell me about..."
```

### `model_resolver.py` - Model Path Resolution

Resolves model paths from common locations:
- `~/.lmstudio/models/`
- `~/GitHub/GITHUB/Hermes/models/`
- `~/.ollama/models/`

```python
from model_resolver import resolve_model_path
model_path = resolve_model_path()  # Auto-detects available models
```

## 📊 Model Benchmarking

This project includes comprehensive benchmarking tools for comparing models:

```bash
# Run benchmarks on a specific model
cd python_hermes_agent
python benchmark/run_benchmark.py --model qwen3.5-9b

# Compare multiple models
python benchmark/batch_benchmark.py --models qwen3.5-9b,gemma4-e2b,llama3.2-3b

# View results
cat ~/.hermes/benchmarks/results.json | jq
```

### Benchmark Categories

- **Speed** - Tokens/sec, latency
- **Accuracy** - Factual correctness
- **Coding** - Code generation, debugging
- **Reasoning** - Math, logic puzzles
- **Tool Use** - Function calling, file operations

## 🔧 Configuration

### Basic Setup (`workspace/config.yaml`)

```yaml
model:
  provider: llama-cpp
  default: local/lmstudio/Qwen3.5-9B-Q4_K_M

agent:
  max_turns: 90
  gateway_timeout: 1800
```

### Environment Variables

```bash
# Model path (required for llama-cpp)
export LLAMA_CPP_MODEL_PATH=/path/to/model.gguf

# GPU layers (optional, default: 35)
export LLAMA_CPP_GPU_LAYERS=35

# Context size (optional, default: 8192)
export LLAMA_CPP_CTX_SIZE=8192

# Hermes home directory
export HERMES_HOME=~/.hermes
```

## 📁 Directory Guide

### `python_hermes_agent/`

All Hermes Agent code lives here - this is what gets duplicated for new instances.

| Subdirectory | Purpose |
|--------------|---------|
| `upstream/` | Original Hermes Agent source (don't modify) |
| `runtime_framework/` | Multi-engine inference support |
| `workspace/` | Your instance data (config, memories, logs) |
| `skills/` | Agent capabilities and tools |
| `benchmark/` | Model testing and comparison tools |

### `~/.hermes/`

User-specific data (NOT duplicated with agent):

| Directory | Purpose |
|-----------|---------|
| `config.yaml` | Active configuration |
| `memories/` | Conversation history |
| `logs/` | Runtime logs |
| `benchmarks/` | Test results |
| `sessions/` | Active session data |

## 🧪 Testing Local Models

### Quick Tests

```bash
# Test model loading
hermes chat -q "Say hello"

# Test tool use
hermes chat -q "List files in current directory"

# Test reasoning
hermes chat -q "What's the square root of 144?"
```

### Comprehensive Testing

```bash
# Full benchmark suite
python python_hermes_agent/benchmark/comprehensive_benchmark.py

# Specific category
python python_hermes_agent/benchmark/comprehensive_benchmark.py --category coding
```

## 🤝 Contributing

This is a **test project** for Hermes Agent with local model extensions. Contributions welcome:

1. **Engine Support** - Add new inference backends (vLLM, tensorrt-llm, etc.)
2. **Model Testing** - Benchmark new GGUF models
3. **Performance** - Optimize local inference speed
4. **Documentation** - Improve setup guides and troubleshooting

## 📝 License

- **Hermes Agent**: Original license applies (see `python_hermes_agent/upstream/LICENSE`)
- **Extensions**: MIT License
- **Models**: Respective model licenses (check each model's license)

## 🔗 Resources

- **Hermes Agent Docs**: https://hermes-agent.nousresearch.com/docs
- **llama.cpp**: https://github.com/ggerganov/llama.cpp
- **MLX**: https://github.com/ml-explore/mlx
- **GGUF Models**: https://huggingface.co/models?library=gguf

## 🐛 Troubleshooting

### Common Issues

**"Unknown provider 'llama-cpp'"**
```bash
# Make sure llama-cpp-python is installed
pip install llama-cpp-python

# Set model path
export LLAMA_CPP_MODEL_PATH=/path/to/model.gguf
```

**"API key required" error**
```bash
# llama-cpp doesn't need an API key - this is a bug
# Set a dummy key as workaround
export LLAMA_CPP_API_KEY=not-needed
```

**Model loads slowly**
```bash
# Increase GPU layers for Metal acceleration
export LLAMA_CPP_GPU_LAYERS=99

# Or reduce context size
export LLAMA_CPP_CTX_SIZE=4092
```

## 📧 Support

For issues specific to this local model extension:
- Open an issue on GitHub
- Check the `docs/` directory for guides

For Hermes Agent core issues:
- Visit https://hermes-agent.nousresearch.com/docs
- Join the Nous Research Discord

---

**Built with ⚕️ Hermes Agent + 🦙 llama.cpp**

*Last updated: May 2026*

---

**Built by [Jenkins Robotics](https://www.youtube.com/@Jenkins_Robotics).**

[YouTube](https://www.youtube.com/@Jenkins_Robotics) · [Patreon](https://www.patreon.com/JenkinsRobotics) · [Discord](https://discord.gg/sAnE5pRVyT) · [GitHub](https://jenkinsrobotics.github.io)
