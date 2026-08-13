# Agent Self-Evolution — Failure-Driven Skill Optimization

A reproducible, low-cost demo of an **Agent self-improvement loop**: the main agent fails during conversation → failure samples are injected into a background Reviewer → minimal Skill `create`/`patch` operations → the Skill library updates → subsequent answers directly use the new Skill. **"Fail-then-train"** without fine-tuning.

Inspired by Anthropic Hermes' Skill Nudge mechanism and Stanford DSPy's GEPA offline-optimization idea, applied to a fictional e-commerce customer-service scenario with fully controlled ground truth.

## Key Results

| Metric | Value |
|--------|-------|
| Accuracy improvement | **22% (baseline) → 70-90%** (after Nudge-driven evolution, rule-based eval) |
| Evolution rounds | Up to 8 Nudge rounds per category block (10 questions each); fully-correct blocks auto-skip |
| Skill library growth | 2 initial Skills → 5-8 after evolution |
| Evaluation set | 60 questions × 6 categories + 30-question fixed Probe subset for cross-version comparison |
| Version tracking | 3-layer persistence: active version + full history JSON + per-version Markdown snapshot |
| Failure taxonomy | 3 mutually-exclusive failure causes (evasion / missing keyword / forbidden word) |
| Code size | ~1,200 lines Python + ~700 lines HTML/JS |
| Run cost | ~200 LLM calls per full experiment, **< 1 RMB** (no sandbox dependency) |

## What Makes This Interesting

1. **Contract-based Agent-Reviewer-Evaluator design** — the Agent answers concretely when it can, or honestly says "please contact customer service"; the Evaluator enforces the contract with one-vote veto + required/forbidden keyword checks. Failure causes are mutually exclusive, so optimization signals are unambiguous.
2. **Runtime Nudge loop** — every block of 10 dialogues, failed turns are routed to a Reviewer that emits the **minimal necessary** Skill changes (only fix observed failures, 1-2 categories per round), leaving an evolution gradient.
3. **Robust rule evaluation** — keyword matching + thousand-separator normalization (`4,000` → `4000`) + negation-prefix detection (`不可直接取消` doesn't false-positive on `直接取消`), keeping rule-based eval stable within its accuracy ceiling.
4. **Visualizable** — FastAPI + SSE streaming + single-file offline HTML frontend; every Nudge event, Skill version diff, and per-question answer change is inspectable.

## Tech Stack

Python · OpenAI SDK · DeepSeek API · FastAPI · SSE · Vanilla JS (no CDN, offline-capable)

## Quick Start

```bash
pip install -r requirements.txt

export DASHSCOPE_API_KEY="sk-xxx"   # or any OpenAI-compatible key (DeepSeek supported)

# Web stepping demo (recommended)
python serve.py

# Headless batch run
python src/demo_runner.py
```

## Project Structure

```
├── src/
│   ├── agent.py                  # Main agent with Skill retrieval
│   ├── background_reviewer.py    # Failure-driven Skill reviewer (minimal-change policy)
│   ├── evaluator.py              # Contract-based rule evaluator (3-class failure taxonomy)
│   ├── skill_manager.py          # 3-layer Skill version tracking + snapshots
│   ├── demo_runner.py            # Headless evolution experiment
│   └── rule_eval_with_review.py  # Rule-eval + review pipeline
├── skills/                       # Evolvable Skill library (refund, vip_benefits, ...)
├── data/
│   ├── demo_script.json          # 80-question script, 8 blocks × 10
│   ├── eval_set.json             # 60-question evaluation set
│   └── policies.md               # Fictional e-commerce policies (ground truth)
├── outputs/
│   ├── evolution_log.json        # Full evolution log with per-question answer comparison
│   ├── experiment_result.json    # Baseline vs evolved accuracy
│   ├── rule_eval_full.json       # Detailed rule-eval results
│   └── skill_snapshots/          # Per-version Skill snapshots
├── skill-optimization-benchmark/  # Skill-optimization benchmark workspace (2 iterations, 3 evals)
├── serve.py                      # FastAPI + SSE web demo
├── index.html                    # Single-file offline frontend
├── ARCHITECTURE.md
└── USAGE_GUIDE.md
```

## License

MIT
