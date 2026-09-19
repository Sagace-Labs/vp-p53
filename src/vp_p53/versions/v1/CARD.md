# p53 v1

_Generated from `manifest.toml` and `metrics.json`. Do not edit._

Released 2026-09-19 · signature 1

**Why this version.** first release: binary XGBoost on Morgan, MACCS and RDKit descriptors over the Tox21 p53 screen

## Outputs

| column | dtype | range | meaning |
|---|---|---|---|
| `p53_activation` | float32 | 0.0–1.0 | P(activates the p53 response element reporter in the Tox21 qHTS screen). The assay does not separate the insults that reach it |
| `p53_cytotox` | float32 | 0.0–1.0 | P(reduces viability in the counter-screen over the same library). A compound scoring high on both readouts raised the reporter in a cell that was also dying |

Missing values: NaN when RDKit cannot parse the input SMILES

## Performance

Protocol `scaffold-balanced-5seed@1` — Bemis-Murcko scaffold split with scaffold groups permuted by seed and each group placed in the fold it overfills least, so a group larger than a fold's capacity settles in train instead of starving that fold. Same fold fractions, seeds and metrics as scaffold-shuffle-5seed@1; only the packing differs. Five seeds; report mean and standard deviation over the held-out test folds.

Evaluated 2026-09-19 on n_train=4440, n_val=531, n_test=796.

### `p53_activation`

| metric | mean | std | per seed |
|---|---|---|---|
| auc_roc | 0.816 | 0.018 | 0.834, 0.840, 0.812, 0.798, 0.798 |
| auprc | 0.411 | 0.055 | 0.451, 0.469, 0.327, 0.364, 0.444 |
| mcc | 0.310 | 0.022 | 0.315, 0.312, 0.296, 0.280, 0.347 |
| brier | 0.096 | 0.012 | 0.083, 0.094, 0.100, 0.086, 0.118 |

### `p53_cytotox`

Measured on n_train=4356, n_val=512, n_test=773.

| metric | mean | std | per seed |
|---|---|---|---|
| auc_roc | 0.805 | 0.044 | 0.788, 0.841, 0.851, 0.817, 0.728 |
| auprc | 0.303 | 0.100 | 0.334, 0.473, 0.298, 0.213, 0.197 |
| mcc | 0.276 | 0.096 | 0.323, 0.438, 0.249, 0.203, 0.168 |
| brier | 0.063 | 0.035 | 0.045, 0.052, 0.045, 0.040, 0.133 |

> Comparable only with metrics carrying the same protocol id.

## Data

Tox21 p53 signalling qHTS — PubChem BioAssay AID 651631 (qHTS assay for small molecule agonists of the p53 signaling pathway) and AID 651633 (qHTS assay for small molecule agonists of the p53 signaling pathway - cell viability), rows called Active or Inactive, one row per compound labelled by majority call across its assay records. Retrieved 2026-09-06, licensed public-domain, redistributed here.

`5767` compounds, positive rate `0.077`, table SHA-256 `ba1a96e935c2acc9…`

Regenerate and check for upstream drift with `python -m vp_p53.data fetch --verify`.

## Model

xgboost-binary on `combo3` features. Shipped weights: one model per output, each on every compound its endpoint labels minus a 10% scaffold carve used for early stopping

`weights.joblib` SHA-256 `e3691a53994f7048…`

## Provenance

Environment: python 3.11.11, rdkit 2026.03.5, xgboost 3.2.0.

Reproducibility is to this dataset hash and this environment, not bit-exact: the sources are live endpoints and RDKit descriptor values move between releases.
