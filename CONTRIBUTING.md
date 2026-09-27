# Contributing

Keep the original notebook and saved outputs intact when making documentation changes. For new modelling work, create a separate notebook or script and identify the exact input data, split, label mapping and dependency versions.

Use a topic branch, stage only the intended changes, then run `python3 tools/check_repository.py` from the repository root. This is a static check and does not execute the experiments.

Keep recordings, environments, caches and credentials out of commits. Describe actual validation in pull requests and distinguish reproduced results from historical outputs. See the [reproduction notes](docs/REPRODUCIBILITY.md) before extending the analysis.
