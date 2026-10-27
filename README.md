# NBA post-miss criticality study

This directory contains the frozen analysis contract and reproducible pipeline for the study **Does Game Criticality Moderate Post-Miss Shot Selection in Professional Basketball?**

## Pilot and confirmation samples

The 2022–23 and 2023–24 seasons are exploratory pilot data because their preliminary effects were inspected before this contract was written. The 2016–17 through 2021–22 seasons are the held-out confirmation sample. Season identifiers use the season start year.

The confirmatory primary outcome is shot distance in feet. The main lag is the same player's previous field-goal attempt within the same game and period; it never crosses a period boundary. Primary criticality means period 4, no more than 300 seconds remaining, and an absolute pre-shot margin no greater than 5 points.

## Confirmation firewall

The pipeline must run in this order:

1. Download the named raw files and produce outcome-blind integrity audits.
2. Develop and test event reconstruction, feature construction, and estimators with synthetic fixtures and the two pilot seasons only.
3. Hash the configuration, analysis source, and tests into a freeze manifest.
4. Verify the freeze manifest in a fresh process.
5. Only then expose H1/H2 coefficients for the six confirmation seasons.

Before the freeze, row counts, schemas, missingness, joins, duplicate keys, dates, score consistency, and other outcome-blind audit results may be computed for confirmation seasons. Coefficients, contrasts, model summaries, plots, or any other result involving `PreviousPersonalMiss`, `Critical`, or their interaction may not be computed or displayed for those seasons. Pilot models are allowed and must be labelled exploratory.

Changing a frozen definition after confirmation results become visible creates a new version and must be disclosed as post-confirmation analysis. Confirmation artifacts are immutable inputs to later pooled and robustness analyses.

## Data and licensing

The input manifest points to the public `shufinskiy/nba_data` repository, whose repository-level license is Apache-2.0. The underlying NBA records may be subject to separate database, website, or contractual rights; therefore raw third-party CSV files remain under `work/` and are not redistributed with the study package. The package publishes download provenance, hashes, code, permitted derived data, and aggregate results.

The PLOS ONE release is assembled by `prepare_plos_submission.py`. It contains six season-separated compressed derived datasets for the held-out confirmation sample, a data dictionary, file hashes, complete coefficient-level supporting tables, and the full analysis code. The release builder rejects provider raw files named `nbastatsv3_*.csv` or `shotdetail_*.csv` and scans the completed release tree before it succeeds.

## Directory roles

- `study_config.json`: immutable scientific definitions and stage permissions.
- `data_manifest.csv`: one row per expected raw file; Task 2 fills URLs, sizes, hashes, and status.
- `src/`: analysis code.
- `tests/`: synthetic and regression tests.
- `results/`, `tables/`, `figures/`, `manuscript/`: generated public artifacts.
- `reproducibility/`: runtime record and attachment instructions.
- `checkpoints/`: SHA-256 task manifests.

The original two-season feasibility script, `../reproduce_nba_post_miss.py`, is preserved unchanged as the exploratory audit trail.

## One-command clean reproduction

Use Python 3.14 and install the versions in `requirements.txt`. Keep the 16 validated provider CSVs outside this release tree, in a directory such as `work/data/raw/`. From the workspace root, run:

```bash
python3 outputs/nba_post_miss_study/run_all.py \
  --raw-dir work/data/raw \
  --version-dir work/reproduction_clean_NEW
```

The version directory must not already exist. The runner validates all raw-file hashes and the analysis freeze, rebuilds audit, pilot, confirmation, internally prespecified robustness, and reporting outputs in the new directory, compares every regenerated artifact byte-for-byte with the sealed release, and refuses to overwrite the original results. It also verifies that raw provider CSVs have not been copied into the public package.

To rebuild the public file manifest after authorized documentation changes:

```bash
python3 outputs/nba_post_miss_study/run_all.py --finalize-only
```

The completed independent run is documented in `reproduction_log.txt`; all 30 compared analytic artifacts were byte-identical.

## Post-confirmation supplementary analyses

The original frozen inputs and sealed confirmation outputs remain unchanged. Reviewer-motivated analyses are versioned separately in `src/post_confirmation.py` and labeled `post_confirmation_supplementary` throughout their JSON and CSV outputs. They add a covariance-preserving six-season refit, alternative criticality constructs, held-out win-probability calibration, multiplicity adjustment, formal timeout heterogeneity, a six/two/eight-season no-timeout decomposition, period and fourth-quarter checks, nonlinear lag-distance adjustment, and shot-zone outcomes. Run the unified revision workflow after the frozen pipeline with:

```bash
python3 outputs/nba_post_miss_study/run_revision.py \
  --raw-dir work/data/raw \
  --cache-dir work/data/analysis_cache
```

The runner revalidates the freeze and raw manifest, protects the two sealed confirmation files by verified hashes, reproduces both six-season and pooled eight-season checks, and validates the persisted supplementary payload. See `reproducibility/README.md` for the attachment contents. A public repository URL or DOI must be added to the manuscript only after the release has actually been deposited; the local files deliberately make no premature public-access claim.
