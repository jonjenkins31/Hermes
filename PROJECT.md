# Hermes Project Structure

This repository contains the **Hermes Agent Local Model Testing Project**.

## What's Here

```
Hermes/
├── python_hermes_agent/     ← ALL Hermes Agent code (copy this for new instances)
├── models/                   ← Optional shared model storage
├── docs/                     ← Documentation
└── README.md                 ← This project overview
```

## Quick Start

1. **Setup**: See `README.md` for installation instructions
2. **Configure**: Edit `python_hermes_agent/workspace/config.yaml`
3. **Run**: `cd python_hermes_agent && hermes chat "Hello"`

## For New Agent Instances

To create a new agent instance:

```bash
# Copy the entire python_hermes_agent directory
cp -r python_hermes_agent python_hermes_agent_v2

# The new instance has:
# ✓ All frameworks (runtime, skills, benchmark)
# ✓ All scripts and tools
# ✗ No user data (create fresh workspace/)
```

## Key Features

- **Multi-engine inference**: llama.cpp, MLX, vLLM support
- **Local-first**: Run completely offline with GGUF models
- **Benchmarking**: Compare cloud vs local models
- **Organized**: Clean separation of code vs user data

See `README.md` for full documentation.
