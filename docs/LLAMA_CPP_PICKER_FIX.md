# llama-cpp Provider Fix Applied ✓

## What Was Fixed

The model picker was showing "(0 models)" for llama-cpp providers because they use `model_path` instead of having a `models:` list in config.

### Patch Applied to `hermes_cli/model_switch.py`

```python
# Added after line 1512:
# For llama-cpp providers with model_path but no models list,
# use the provider name as the model identifier
if model_path and not models_list:
    # The model name is the provider slug itself (e.g., "gemma4-26b")
    models_list.append(ep_name)
```

## How It Works Now

Each llama-cpp provider entry shows:
```
gemma4-26b (1 model)  ← selectable!
qwen3-coder-30b (1 model)
qwen3.5-9b (1 model)
...
```

When you select `gemma4-26b`, it sets:
- `model.provider: llama-cpp`
- `model.default: gemma4-26b`

This tells Hermes to use the llama-cpp provider with the `gemma4-26b` config entry, which points to your GGUF file.

## Test It

```bash
# In your terminal
hermes model
```

Select any of the llama-cpp providers. They should now be selectable (no longer show "0 models").

## Quick Switch Alternative

```bash
# Use the quick switch script
python3 ~/.hermes/scripts/switch_to_local.py llama3.2-3b

# Test it works
hermes chat -q "Hello, are you running locally?"
```

## Files Modified

```
python_hermes_agent/upstream/hermes_cli/model_switch.py
  - Line 1514-1518: Auto-add provider name as model for llama-cpp
```

---

**Status: Ready to test!** Run `hermes model` and select a local model.
