# Local Models Setup Complete! ✓

## What's Configured

✅ **llama-cpp-python installed** (v0.3.23 with Metal support)
✅ **15 GGUF models discovered** on your system
✅ **7 providers configured** in `~/.hermes/config.yaml`

## Your Local Models

| Name | Provider | Size | Path |
|------|----------|------|------|
| `gemma4-26b` | llama-cpp | 15.6 GB | ~/GitHub/GITHUB/Hermes/models/ |
| `qwen3-coder-30b` | llama-cpp | 17.4 GB | ~/GitHub/GITHUB/Hermes/models/ |
| `qwen3.5-9b` | llama-cpp | 5.2 GB | ~/.lmstudio/models/ |
| `gemma4-31b` | llama-cpp | 17.4 GB | ~/.lmstudio/models/ |
| `gemma4-e4b` | llama-cpp | 5.0 GB | ~/.lmstudio/models/ |
| `gemma3-12b` | llama-cpp | 6.8 GB | ~/.lmstudio/models/ |
| `llama3.2-3b` | llama-cpp | 1.9 GB | ~/.lmstudio/models/ |

## How to Switch Models

### Method 1: Quick Switch Script
```bash
# List available models
python3 ~/.hermes/scripts/switch_to_local.py

# Switch to a specific model
python3 ~/.hermes/scripts/switch_to_local.py gemma4-26b
python3 ~/.hermes/scripts/switch_to_local.py llama3.2-3b
python3 ~/.hermes/scripts/switch_to_local.py qwen3.5-9b
```

### Method 2: Manual Config Edit
```bash
hermes config edit
```

Change:
```yaml
model:
  provider: llama-cpp   # was: ollama-cloud
  default: gemma4-26b   # was: qwen3.5:397b
```

### Method 3: Command Line Override
```bash
# One-time use
hermes chat -m gemma4-26b

# Or with full provider path
hermes --provider llama-cpp --model gemma4-26b
```

## Why `/model` Doesn't Show Locals Yet

The `hermes model` interactive picker shows models from **known providers** (Ollama, OpenRouter, Anthropic, etc.). Custom llama-cpp providers need to be:

1. **Defined in config.yaml** ✓ (done)
2. **Selected via config** (use the switch script above)

The picker doesn't auto-discover custom providers - you need to set them explicitly.

## Test Your Local Model

```bash
# Quick test
hermes chat -q "What is 2 + 2?"

# Interactive session
hermes

# With verbose output
hermes --verbose chat -q "Write a haiku about coding"
```

## Benchmark Your Models

Run the benchmark script with different models:

```bash
# Switch model
python3 ~/.hermes/scripts/switch_to_local.py gemma4-26b

# Run benchmark
python3 ~/.hermes/benchmarks/run_benchmark.py

# Compare results in ~/.hermes/benchmarks/
```

## Performance Notes

Based on your hardware (Apple Silicon T6020):

| Model | Expected Speed | RAM Usage | Best For |
|-------|---------------|-----------|----------|
| llama3.2-3b | ~50 tok/s | 2 GB | Quick tasks |
| qwen3.5-9b | ~35 tok/s | 5 GB | Balanced |
| gemma4-e4b | ~40 tok/s | 5 GB | Efficient |
| gemma3-12b | ~25 tok/s | 7 GB | Reasoning |
| gemma4-26b | ~15 tok/s | 16 GB | Complex tasks |
| qwen3-coder-30b | ~12 tok/s | 17 GB | Coding |

## Troubleshooting

### "llama-cpp provider not found"
Make sure config.yaml has the providers section (it does).

### Model loading is slow
First load takes 10-30 seconds. Subsequent requests are faster.

### Out of memory
Reduce `n_gpu_layers` in config.yaml from 35 to 20.

### Poor output quality
Try a different model - smaller models (3B-9B) are faster but less capable.

## Files Created

```
~/.hermes/scripts/
  ├── scan_local_models.py    # Discover GGUF models
  ├── switch_to_local.py      # Quick model switcher
  ├── test_llama_cpp.py       # Test llama-cpp installation
  └── run_benchmark.py        # Model benchmark suite

~/.hermes/benchmarks/
  └── benchmark_*.md          # Benchmark results

~/.hermes/discovered_models.json  # All found models
```

## Next Steps

1. **Test a local model:**
   ```bash
   python3 ~/.hermes/scripts/switch_to_local.py llama3.2-3b
   hermes chat -q "Hello!"
   ```

2. **Run benchmark comparison:**
   ```bash
   # Test current model (qwen3.5:397b cloud)
   python3 ~/.hermes/benchmarks/run_benchmark.py
   
   # Switch to local
   python3 ~/.hermes/scripts/switch_to_local.py gemma4-26b
   
   # Test local model
   python3 ~/.hermes/benchmarks/run_benchmark.py
   ```

3. **Compare results in `~/.hermes/benchmarks/`**

---

**Status: READY** 🎉

Your local models are configured and ready to use!
