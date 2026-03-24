# Experiment Log

| # | Step | Category | Total (μs) | Δ vs prev | Notes |
|---|------|----------|------------|-----------|-------|
| 0 | baseline | — | 1782.62 | — | upstream/develop, old tokenizer architecture |
| 1 | micro-opt — spec.py optimizations independent of parser | misc | 1716.36 | -66.26 | |
| 2 | prereq — add VersionList._from_version_list_string() (no callers yet) | prereq | 1704.15 | -12.21 | |
| 2b | after step 2 re-run | — | 1683.70 | — | re-run to confirm baseline for step 3 |
| 3 | named capture groups + _from_version_list_string | 4, 5 | 1671.55 | -12.15 | subvalues in VERSION/BOOL_VARIANT/KEY_VALUE_PAIR tokens; also tokenize.py __eq__ accepts None subvalues |
| 4 | token consolidation | 4 | 1517.40 | -154.15 | merged VERSION/BOOL_VARIANT/KEY_VALUE_PAIR/DEPENDENCY; tokenize.py now only stores non-None subvalues |
| 5 | skip WS tokens before allocation | 3 | 1510.42 | -6.98 | inline WS skip in tokenize(); remove parseable_tokens() wrapper |
| 6 | inline parser classes + replace accept/expect with direct dispatch | 2 | 1289.36 | -221.06 | inlined SpecNodeParser, EdgeAttributeParser, FileParser into SpecParser; use `is` for kind checks; manual advance |
| 7a | TokenContext drives scanner directly — no Token alloc, no generator | 1 | 966.24 | -323.12 | Replace generator+Token with scanner+re.Match in TokenContext; dispatch via lastgroup/group(); _parse_edge_attrs returns (attrs, substitute) |
| 7b | FAST_SPEC_REGEX + flatten ctx to curr/next | 2 | 875.54 | -90.70 | \s* prefix for C-level WS; drop TokenContext; SpecParser holds curr/next inline; cache Spec class in _Spec |
| 7c | reorder declarations to match reference commit | — | 863.45 | -12.09 | pure reorder + conf.py import fix; no functional changes; parity with ea96bb688 |
