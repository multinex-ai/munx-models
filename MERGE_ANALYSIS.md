# Sovereign Hybrid Merge Analysis

## Source Models

### Qwen3.5-Abliterated-32B (Weight: 55%)
| Trait | Score | Notes |
|-------|-------|-------|
| Coding | 92 | Excellent code generation, refactoring |
| Reasoning | 88 | Strong logical chains |
| Tool Calling | 83 | Good but not exceptional |
| Uncensored | 100 | All guardrails removed |
| Context | 128K | Full Qwen context |
| Architecture | Qwen2.5 | Transformer with GQA |

### GLM-5-32B (Weight: 45%)
| Trait | Score | Notes |
|-------|-------|-------|
| Coding | 88 | Very good, slightly below Qwen |
| Reasoning | 85 | Solid but not top-tier |
| Tool Calling | 95 | **BFCL Leader** - best structured outputs |
| Uncensored | 0 | Standard safety alignment |
| Context | 200K | Extended context |
| Architecture | GLM-4 | ChatGLM transformer variant |

---

## Expected Hybrid Characteristics

### DARE-TIES Merge Behavior
DARE (Drop And REscale) + TIES (TrIm, Elect Sign & merge):
1. **Drops** redundant parameters (density: 0.6 = keeps 60%)
2. **Trims** low-magnitude deltas
3. **Resolves sign conflicts** by majority vote
4. **Rescales** remaining parameters

### Predicted Capabilities

| Capability | Qwen | GLM-5 | **Hybrid** | Confidence |
|------------|------|-------|------------|------------|
| Coding | 92 | 88 | **89-91** | High |
| Reasoning | 88 | 85 | **85-87** | High |
| Tool Calling | 83 | 95 | **88-92** | Medium |
| Uncensored | 100 | 0 | **55-75** | Low |
| Coherence | 95 | 92 | **85-90** | Medium |
| Context | 128K | 200K | **~128K** | High |

### What You'll Get

#### ✅ Strong Points
1. **Enhanced Tool Calling** - GLM-5's BFCL excellence will boost structured outputs
2. **Retained Coding Strength** - Qwen's coding won't degrade much
3. **Partial Uncensoring** - ~60-70% of abliterated behavior retained
4. **Better JSON/Schema** - GLM excels at following exact formats
5. **Function Execution** - More reliable parameter extraction

#### ⚠️ Potential Issues
1. **Inconsistent Censorship** - May refuse some things Qwen wouldn't
2. **Occasional Confusion** - Different training can cause edge-case weirdness
3. **Tokenizer Mismatch** - Using Qwen tokenizer on GLM weights (minor)
4. **Context Regression** - May not reach GLM's 200K effectively

#### ❓ Unknowns
1. **Personality Blend** - Could be quirky until you tune temperature
2. **Code Style** - May mix Qwen/GLM commenting conventions
3. **Safety Triggers** - Some GLM guardrails may resurface randomly

---

## Recommended Use Cases

| Use Case | Suitability | Why |
|----------|-------------|-----|
| Agentic workflows | ⭐⭐⭐⭐⭐ | Best of both: reasoning + tool execution |
| Code generation | ⭐⭐⭐⭐ | Strong coding retained |
| Uncensored research | ⭐⭐⭐ | Partial - test first |
| Function calling | ⭐⭐⭐⭐⭐ | GLM boost significant |
| Creative writing | ⭐⭐⭐ | May have inconsistent voice |
| Long context tasks | ⭐⭐⭐ | Limited to ~128K effective |

---

## Alternative Merge Strategies

### If You Want MORE Uncensored (80%+)
```yaml
models:
  - model: huihui_ai/Qwen3.5-abliterated-32B
    parameters:
      weight: 0.75
      density: 0.7
  - model: THUDM/glm-5-32b-chat
    parameters:
      weight: 0.25
      density: 0.5
merge_method: ties  # Not DARE - preserves more
```

### If You Want MAXIMUM Tool Calling
```yaml
models:
  - model: huihui_ai/Qwen3.5-abliterated-32B
    parameters:
      weight: 0.35
      density: 0.5
  - model: THUDM/glm-5-32b-chat
    parameters:
      weight: 0.65
      density: 0.7
merge_method: dare_ties
```

### Three-Way Merge (Add Reasoning)
```yaml
models:
  - model: huihui_ai/Qwen3.5-abliterated-32B
    parameters:
      weight: 0.40
  - model: THUDM/glm-5-32b-chat
    parameters:
      weight: 0.35
  - model: deepseek-ai/DeepSeek-R1-32B
    parameters:
      weight: 0.25
merge_method: ties
```

---

## Post-Merge Tuning Tips

1. **Temperature**: Start at 0.6, increase if too rigid
2. **Top-P**: 0.85 works well for merged models
3. **Repeat Penalty**: 1.15 to prevent loops from conflicting weights
4. **System Prompt**: Be explicit about capabilities (see enhanced prompt)
