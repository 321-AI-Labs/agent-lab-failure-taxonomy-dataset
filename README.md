# Failure Taxonomy of an Agent-Run Research Lab

105 documented process failures from one agent-run research lab, classified by
pipeline phase, cause class, and which governance layer caught them. Sealed
dataset v1.1, an audited taxonomy, and an open submission format.

- Study page: https://321ai.xyz/studies/agent-lab-failure-taxonomy-dataset/
- Report PDF: https://321ai.xyz/studies/agent-lab-failure-taxonomy-dataset/TR5-failure-taxonomy.pdf
- Publisher: 321 AI Labs (https://321ai.xyz)

## What's here

`dataset/` holds the sealed v1.1 release, byte-identical to the lab's frozen
artifacts:

- `records.jsonl` - the 105 sealed records (sha256 6c10e35767fceaadc5091e745876293b702f66d33062c31267547b6ef510a3ee)
- `dataset-metadata.json` - counts, provenance, license
- `taxonomy-schema.json` - taxonomy v1.2 controlled vocabularies and decision rules
- `kappa-audit-trail.json` - the three-iteration blind dual-classification audit (0.7282/0.4580/0.9459 -> 0.9649/1.0/1.0), sealed label hashes, the P5-F-043 adjudication
- `kappa-input-sample.jsonl` - the 40-record stratified audit sample (seed 20260914)
- `README.md` - the dataset card (census caveats quoted from it everywhere the zeros appear)
- `SUBMISSION-FORMAT.md` - how external labs contribute records without breaking the seal
- `VERSION` - 1.1, with changelog

## Headline numbers

- 105 records; source families flashnext 33, sprint-review 60, timesfm 7, job-killer 5; window September 11-15, 2026
- Phases: build 35, verify 27, ops 25, spec 18
- Top cause: verification_gap, 46 of 105 (44%) - the check existed but was never run
- Detection: self_caught 72 (69%), reviewer_caught 33 (31%), owner_caught 0, escaped 0 - both zeros carry the dataset card's census caveat
- Cost coverage: 3 measured / 1 estimate / 101 null, so no cost distribution is reported

## License

Research use only. See `dataset/README.md` and `dataset/dataset-metadata.json`
for the license note and per-record provenance.

## Citation

See `CITATION.cff`. The report DOI backfills here once Zenodo mints it.
