# Experiment Log

| # | Step | Category | Total (μs) | Δ vs prev | Notes |
|---|------|----------|------------|-----------|-------|
| 0 | baseline | — | 1782.62 | — | upstream/develop, old tokenizer architecture |
| 1 | micro-opt — spec.py optimizations independent of parser | misc | 1716.36 | -66.26 | |
| 2 | prereq — add VersionList._from_version_list_string() (no callers yet) | prereq | 1704.15 | -12.21 | |
| 2b | after step 2 re-run | — | 1683.70 | — | re-run to confirm baseline for step 3 |
| 3 | named capture groups + _from_version_list_string | 4, 5 | 1671.55 | -12.15 | subvalues in VERSION/BOOL_VARIANT/KEY_VALUE_PAIR tokens; also tokenize.py __eq__ accepts None subvalues |
| 4 | token consolidation | 4 | 1517.40 | -154.15 | merged VERSION/BOOL_VARIANT/KEY_VALUE_PAIR/DEPENDENCY; tokenize.py now only stores non-None subvalues |
