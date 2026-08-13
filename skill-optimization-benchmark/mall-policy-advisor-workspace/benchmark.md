# Benchmark summary

| Metric | v1 original | v2 optimized | Change |
| --- | ---: | ---: | ---: |
| Assertion pass rate | 100.0% | 100.0% | +0.0% |
| Skill characters | 2603 | 1611 | -38.1% |
| Skill lines | 142 | 65 | -77 |
| Mean token proxy per run | 2363.3 | 1390.3 | -41.2% |

Token proxy = ceil(UTF-8 bytes / 3). It supports a controlled relative comparison for Chinese text, not billing-token reconstruction.
