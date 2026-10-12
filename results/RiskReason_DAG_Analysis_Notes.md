# RiskReason: DAG-derived empirical analysis (October 2026)

## Sources and scope

- Literal graph dataset: `Financial_Reasoning_Benchmark-17_DAG_Complete_Batch21.xlsx` (206 records, 4,995 nodes, 6,724 edges).
- Grades: reviewed nine-model task-level PDF appendices reconstructed in `reviewed_grades_from_pdf_appendices.json`, joined in `RiskReason_DAG_ModelTask_Joined.csv`.
- Eight full-regime models: 1,638 evaluated model-task observations on 205 unique tasks; 1,398 observations on 175 Standard tasks and 240 on 30 long-horizon tasks.
- DeepSeek: 120 risk tasks only; all nine models have a common 118-risk-task subset (1,062 observations). DeepSeek is not included in the full-regime analyses.

## Descriptive results, Standard tasks, eight full-regime models

| Bin | Number of tasks | Passes / responses | Exact pass |
|---|---:|---:|---:|
| Longest path 0–5 edges | 96 | 497/766 | 64.88% |
| Longest path 6–9 edges | 61 | 278/488 | 56.97% |
| Longest path >=10 edges | 18 | 76/144 | 52.78% |
| DAG output nodes 1–3 | 87 | 464/696 | 66.67% |
| DAG output nodes 4–9 | 66 | 300/526 | 57.03% |
| DAG output nodes >=10 | 22 | 87/176 | 49.43% |

Graph longest-path depth and DAG terminal-output count are positively correlated (task-level Spearman rho = 0.479). DAG output counts differ from scored-field counts for 62 of the 205 evaluated tasks.

## Exploratory adjusted associations

All models below use binomial GLM with task-clustered robust standard errors, model fixed effects, and (for analyses including both risk and legacy probe tasks) task-family fixed effects. Odds ratios (OR) are descriptive, not causal.

| Population/specification | Depth OR (95% CI) | Output-complexity OR (95% CI) |
|---|---|---|
| Standard, depth only | 0.920 (0.854–0.991) | — |
| Standard, depth + log(1 + DAG outputs) | 0.950 (0.884–1.020) | 0.281 (0.176–0.448) |
| Standard, depth + log(1 + graded fields) | 0.945 (0.872–1.024) | 0.399 (0.250–0.637) |
| Standard, parse-valid rows only | 0.963 (0.892–1.040) | 0.283 (0.168–0.475) |
| Standard, five higher-scoring models | 0.973 (0.895–1.057) | 0.257 (0.140–0.472) |
| Standard, DAGs labeled validated or validated_macro | 0.932 (0.869–1.000) | 0.345 (0.199–0.600) |
| All 205 tasks, eight full-regime models | 0.983 (0.926–1.044) | 0.311 (0.197–0.490) |
| Common 118 risk tasks, all nine models | 0.955 (0.884–1.032) | 0.289 (0.165–0.505) |

The output-complexity estimates are per one-unit increase in `log(1 + number of outputs)`, *not per additional output node*. They should not be interpreted as isolated cognitive difficulty effects; strict all-fields-correct scoring gives longer answer contracts more opportunities for failure.

## DAG-aligned evidence

- `probe-036`, Gemini 3.1 Pro Preview: correct posterior PD 13.3929%, stressed PD 23.6220%, LGD 60%, and ECL $1.134M; incorrectly tests a 15% monitoring limit against posterior PD instead of stressed PD. The first *observed* divergence is at the monitoring condition, after correct probability computations.
- `long-012`, Phi-4: correctly identifies principal loss waterfall and trigger, then increases senior balance with $12M principal collection instead of reducing from $70M to $58M; downstream interest allocation differs.
- `long-029`, Phi-4: proposed asset-sale mix is infeasible under the 6% CET1/RWA requirement (actual ~5.61%); the optimization/remediation solution is therefore invalid, beyond mere numeric imprecision.
- `long-025`, Gemini Pro/Flash: the reviewed record indicates the principal 40 financial state fields are correct but three event descriptions are different or omitted. Non-pass need not be a numerical state-reasoning failure.
- `risk-120`: all nine fail exact grading, with Pro producing null AIC/BIC and Flash incorrect numeric estimates; reference AIC/BIC values are not independently recomputed and some requested lag-selection outputs are unscored. Avoid claims of independently proven numeric disagreement until audited.

## Grading sensitivity and limitations

- The original JSON grades were available for seven models; for the two Gemini models, complete raw responses were not part of the reviewed evidence. Task-level reviewed grades for all nine came from PDF appendices.
- Parsing status recorded for eight models is uneven, especially for the two 7B models. Parse-valid-only sensitivity is a **selected** subset, not a corrected accuracy rate.
- DAGs are structurally acyclic, but some have compressed `series_macro` nodes; graph depth is not invariant to representation granularity.
- Five DAGs require reference adjudication: `risk-005`, `risk-034`, `risk-046`, `risk-063`, `risk-092`; others with caveats include `risk-117`, `risk-125` and `risk-120`.
- Observed DAG error localization applies only when intermediate responses have been reviewed; task-level scores alone do not show internal reasoning paths.

## Manuscript placement

Add `\usepackage{booktabs,tabularx}` in the preamble. Insert the three subsections from `RiskReason_DAG_Empirical_Sections.tex` into Evaluation Methodology, Results, and Failure Analysis. Insert the limitations paragraph into Limitations; include the optional `probe-036` DAG visualization as a `figure*` near the first DAG-aligned case. All numbers are descriptive/exploratory and should be frozen only after final reference adjudication and grade-source verification.
