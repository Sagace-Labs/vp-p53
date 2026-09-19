# vp-p53

Predicts activation of the p53 genotoxic stress response from a SMILES string
— a molecular initiating event for drug-induced liver injury, where a genotoxic
insult stabilises p53 and it transactivates its pro-apoptotic target genes.

## Install

    pip install vp-p53

## Use

    from vp_p53 import predict
    predict(["CC(=O)Oc1ccccc1C(=O)O"])   # -> DataFrame[p53_activation, p53_cytotox]

Returns one row per input and one column per declared output. `p53_activation`
is the probability of raising the p53 response element reporter; `p53_cytotox`
is the probability of reducing viability in the counter-screen over the same
library. A compound scoring high on both raised the reporter in a cell that was
also dying. Several distinct insults reach the endpoint and the assay does not
separate them. Unparseable SMILES come back as NaN. Pin a version with
`predict(smiles, version="v1")`; list what is available with `versions()`.

## Current version

**v1**, signature 1, measured under protocol `scaffold-balanced-5seed@1`.
The full record — metrics per output and per seed, dataset hash,
environment — is in
[`src/vp_p53/versions/v1/CARD.md`](src/vp_p53/versions/v1/CARD.md).

## Data

PubChem BioAssay AID 651631, the Tox21 qHTS screen for agonists of the p53
signalling pathway, and AID 651633, its cell-viability counter-screen over the
same library. Both are reduced to one row per compound labelled by the majority
call across its assay records, retrieved 2026-09-06 and redistributed here as a
United States government work in the public domain. Rebuild and check for
upstream drift with `python -m vp_p53.data fetch --verify`; see
[`data/README.md`](data/README.md) for the expected layout.

## Retrain

    python -m vp_p53.train --version v2 --reason "why this version exists"
    python -m vp_p53.evaluate --version v2

`train` fits one deployment model per output on the whole dataset and writes a
new version directory; `evaluate` refits per seed under the protocol and
records what those held-out models scored. Reproducibility is to the recorded
dataset hash and environment, which can change.

## Licence

Code is Apache-2.0 ([`LICENSE`](LICENSE)). The bundled dataset is in the public
domain ([`LICENSE-DATA`](LICENSE-DATA)).

## Cite

See [`CITATION.cff`](CITATION.cff).
