# NBA post-miss criticality study

Public data-and-code deposit for **Does game criticality moderate post-miss shot selection in professional basketball? Evidence from multi-season NBA data**.

## Files in this repository

- `derived_confirmation_2016.csv.gz` through `derived_confirmation_2021.csv.gz`: season-separated derived confirmation data.
- `derived_data_dictionary.csv`: definitions, units, coding, derivations, and missing-data rules for every released field.
- `derived_data_manifest.csv`: row counts, file sizes, and SHA-256 hashes.
- `nba_post_miss_code_results_v2_portable.zip`: portable code-and-results archive. This corrected archive supersedes the earlier package while preserving the original freeze manifest.

SHA-256 (`nba_post_miss_code_results_v2_portable.zip`): `f8e15b667ffaeeb8c06d3887a7f49d090cbdf0193440b241c21be42f558f12f1`.

## Reproduce the analysis

1. Download and extract `nba_post_miss_code_results_v2_portable.zip`.
2. Keep `SCI_research_design_post_miss_criticality_zh.md` beside the extracted `nba_post_miss_study` folder. The archive already has this layout.
3. Obtain the third-party raw files listed in `nba_post_miss_study/data_manifest.csv`; verify their recorded byte sizes and SHA-256 hashes. Raw provider files are not redistributed here.
4. From the extracted top-level folder, run:

   `python3 nba_post_miss_study/run_all.py --raw-dir /absolute/path/to/raw --version-dir /absolute/path/to/new-output`

The version directory must not already exist. The runner validates the raw files and the unchanged freeze record before rebuilding the sealed analysis. `nba_post_miss_study/reproduction_log.txt` records the completed local verification supporting the manuscript.

## Licensing and data rights

The source code is released under the repository's Apache-2.0 license. The underlying NBA-derived records may be subject to separate rights or source terms; they are not redistributed. The released derived analysis data, provenance, and code are provided to support verification and reproduction of the reported results.
