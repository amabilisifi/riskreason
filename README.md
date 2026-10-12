---
license: cc-by-4.0
language:
- en
pretty_name: Financial Risk Reasoning Benchmark
task_categories:
- question-answering
tags:
- finance
- risk
- reasoning
- benchmark
configs:
- config_name: standard
  data_files: data/standard.csv
- config_name: probes
  data_files: data/probes.csv
- config_name: long_horizon
  data_files: data/long_horizon.csv
- config_name: task_annotations
  data_files: data/task_annotations.csv
---

# Financial Risk Reasoning Benchmark

Financial math and risk reasoning tasks covering returns, variance, VaR, credit risk,
actuarial math, bond math and more. Each task has a prompt, a ground-truth answer, a formal
notation and difficulty annotations.

## Subsets

| Config | Rows | What it is |
|---|---|---|
| `standard` | 120 | Single-scenario tasks (`risk-*`). |
| `probes` | 55 | Tasks built to separate models (`probe-*`). Each has a `failure_mode_target`, a `paired_probe_id` and a `diagnostic_pass_condition`. A probe passes only if the answer value matches **and** the pass condition holds. |
| `long_horizon` | 30 | Multi-step scenarios (`long-*`). Each chains two or more standard tasks (`source_task_ids`) and carries a `dependency_graph`, `graph_depth`, `branch_count` and `intermediate_variables`. |
| `task_annotations` | 239 | Normalized metadata for every task: domain group, reasoning family, recomputed difficulty and QA flags. |

```python
from datasets import load_dataset
ds = load_dataset("<user>/<repo>", "standard")
```

## Key fields

- `User Prompt (Input)`: the prompt sent to the model.
- `Ground Truth (Expected)`: the expected answer, usually JSON. A few answers are written as
  Python code that has to be run to get the expected value.
- `formal_notation` and `reasoning_DAG`: the math, and the reasoning steps as a graph.
- `*_score` and `difficulty_score`: difficulty annotations.

## Grading

To compare a model's answer with the ground truth:
- Normalize units. Models switch between percent and decimal (1.0 vs 0.01).
- Treat yes/no and true/false as the same answer.
- Compare numbers with a tolerance, not exact string match.

The full grader is in the code repo: <GITHUB_LINK>.

## Known issues

- `risk-079`: the prompt is ambiguous about whether the 15% volatility is the position's or
  the S&P's.
- `risk-053`: the answer may be swapped. This is not resolved yet.
- `risk-013`: assumes a 256-day trading year, which is a US convention.

## Reference results

`results/` holds graded runs for GPT, Gemini, DeepSeek-R1-Distill, Phi-4, Fin-o1, DianJin and
Qwen2.5. `results/comparison.md` has the summary table. `results/revised/` holds the same runs
with partial-credit cases reviewed by hand.

`gpt-6-luna` was run through a third-party proxy that adds a hidden system prompt of about 6k
tokens. Treat its scores as indicative only.

## Citation

