# P5 Failure Dataset — Dataset Card (v1.0)

> Envelope: `{"artifact_type":"failure-dataset-card (extension declared per E-P5-1)","created_utc":"2026-09-15T16:35:00Z","agent":"GLM (Worker-1, P5 extraction)","mission_brief":"BRIEF-20260914-sprint-p5-failure-taxonomy.md","inputs":{"repos":[],"scripts":[{"path":"glm/work/p5/tools/build_dataset.py"}]},"environment":{"os_build":"Windows 10.0.26200 x64 (Git Bash)"},"method":{"see":"dataset-metadata.json envelope (authoritative)"},"data":{"version":"1.0","N":105},"provenance":{"measured_vs_estimated":"all record content from cited sources; one estimate-flagged cost (P5-F-025)","sample_N":105,"schema_note":"card artifact_type extends the standing enum; declared, escalation E-P5-1"}}

## Identity

- **Name:** P5 Failure Taxonomy of an Agent-Run Lab — failure dataset
- **Version:** 1.1 (see `VERSION`; v1.1 = post-seal label promotion, 2026-09-15)
- **Records:** 105 incidents (`records.jsonl`), one JSON object per line,
  field order fixed, keys sorted, no timestamps in the payload
- **Point of contact:** Worker-1 (GLM) via the P5 mission chain;
  orchestrator: Kimi (`BRIEF-20260914-sprint-p5-failure-taxonomy.md`)

## Provenance

Extracted read-only, 2026-09-15, from four source families of one research
fleet ("Bespoke AI Creation" + sibling projects), Sep 11-15 2026:

1. **flashnext** daily logs (33 records) — `glm/daily/DAILY-2026-09-{11,13,14,15}.md`
2. **timesfm** project index + notes (7 records) — `TimesFM Project/CONTEXT.md`,
   `notes/phase3_pilot_layerA_2026-09-11.md`, `notes/strike_basis_finding_2026-09-14.md`,
   `notes/timesfm3_review.md` (READ-ONLY external project)
3. **job-killer** docs (5 records) — `README.md`, `GLM-PAYLOAD.md`,
   `docs/DETECTOR-CLASSES.md`, `deploy/README.md`,
   `deploy/RESULTS-20260914-hostmode.md`, `scoring/README.md`, `tray/README.md`
   (READ-ONLY external project)
4. **sprint-review** cycle artifacts (60 records) — the fleet's own
   adversarial reviews, data-steward and gatekeeper passes, failed-takes logs,
   OPEN-ISSUES files, and the daily-log failed-take/anomaly/escalation sections

Every record cites `file` + `entry` in its primary source. Numbers enter
records only from those sources. Cost tally: **3 measured** (P5-F-028 30.6 h;
P5-F-040 192 h elapsed degraded, from source dates; P5-F-107 0.5 h session
pause, from source timestamps), **1 estimate** (P5-F-025, 18-30 h — labeled as
such in its source), **101 null**. Note on P5-F-028: the 30.6 h/arm figure is
inherited verbatim from its source (glm/daily/DAILY-2026-09-14.md, Stage 3
DEVIATION D2); the source's own arithmetic 196 s x 576 windows computes to
31.36 h — discrepancy disclosed, the source's stated figure retained. All
other costs are `null` with a `cost_note`, because the sources state no cost.

Build: `tools/build_dataset.py` (deterministic assembler; determinism proof in
`dataset-metadata.json` — anchored to an externally-observed digest preserved
by Reviewer-1 in `review-tmp/det-run1|det-run2/records.jsonl` and re-verified
against the persisted sidecar on every build, plus two same-input runs with
hash-compare).

## Dataset description

What failure modes occur when AI agents run a research operation, at what
rate, and at which pipeline layer they are caught. Phase distribution
(spec 18 / build 35 / verify 27 / ops 25), cause-class distribution
(verification_gap 46, environment_drift 17, spec_drift 16, resource_blindness 12,
retrieval_failure 8, precision_overclaim 4, self_confirmation 1,
credential_exposure 1), detection distribution
(self_caught 72 / reviewer_caught 33 / owner_caught 0 / escaped 0).
These are the promoted v1.1 labels (audit-agreed taxonomy v1.2).

Headline observations (from the v1.0 corpus, not generalizing beyond it):
verification gaps dominate (53%); the adversarial-review layer caught 31% of
failures that the owning worker had not; nothing fully escaped the fleet's
detection layers in this window (0 `escaped` — late detections are recorded
with their delay in cost/notes, e.g. the TimesFM OOM spiral running 8 days
before diagnosis). The `credential_exposure` class was promoted from a
proposed class at audit iteration 1 (dual-classifier agreement 1.0) and its
label applied in records.jsonl at the v1.1 promotion.

## Taxonomy and audit

`taxonomy-schema.json` (v1.2: phase anchored on where the defect lives with
boundary rules R1-R3 and the measured-data-products rule; verification_gap
narrowed with explicit boundary rules; `credential_exposure` a full class)
defines the three controlled vocabularies with one-line decision rules.
records.jsonl carries the audit-agreed v1.2 labels, promoted at dataset v1.1
after Reviewer-1's PASS (iteration-2 kappa: cause_class 0.9649
pre-adjudication, phase 1.0000, detection_layer 1.0000; the single cause
split, P5-F-043, was adjudicated to resource_blindness). The audit is CLOSED
- protocol satisfied in 2 revisions (trajectory 0.728/0.458/0.946 →
0.965/0.792/1.000 → 0.965/1.000/1.000) - fully logged in
`kappa-audit-trail.json`. The dual independent classification protocol
operates on `kappa-input-sample.jsonl` (40 records stratified by
source_project, seed 20260914, stripped to `record_id` + `description` +
`citation` so the second classifier classifies blind).

## Intended uses and limitations

- Feeds mission W1 (recursive governance) and the P5 technical report.
- Suitable for: failure-mode frequency analysis, detection-layer analysis,
  taxonomy calibration studies, agent-fleet process design.
- NOT suitable for: rates "per agent-hour" (the denominators were not
  uniformly instrumented across source projects), benchmarking model
  generators, or any claim that the corpus is a random sample of agent
  failures — it is a census of *documented* failures in four deliberately
  governance-heavy projects, which biases toward well-logged, well-caught
  failures.
- Content note: descriptions quote operational logs and reviews; they contain
  no model weights, no credentials (P5-F-033 describes an exposure incident
  without reproducing the secret), and no personal data beyond pseudonymous
  agent roles.

## License

**Research use only.** The flashnext and sprint-review records are derived
from work involving orcarouter uncensored-weights artifacts (license line per
the standing governance contract, trap #9); the dataset as a whole therefore
carries the "research use only" restriction. TimesFM and Job Killer records
carry no weight-derived content. Contributions under the open submission
format (`SUBMISSION-FORMAT.md`) may set per-record licenses.

## Files

| file | contents |
|---|---|
| `VERSION` | `1.1` + changelog line (this file's envelope lives in dataset-metadata.json) |
| `records.jsonl` | the 105 failure records (audit-agreed v1.2 labels, promoted at v1.1) |
| `dataset-metadata.json` | section-3 envelope (incl. E-P5-1 schema_note) + counts + hardened determinism proof |
| `taxonomy-schema.json` | controlled vocabularies v1.2 + decision rules + promotion log |
| `kappa-input-sample.jsonl` | 40-record blind dual-classification sample |
| `kappa-audit-trail.json` | kappa audit log: iterations 0-2 (all closed, PASS), revisions 1-2, F-043 adjudication, promotion record |
| `SUBMISSION-FORMAT.md` | open submission contract |
| `README.md` | this card |
