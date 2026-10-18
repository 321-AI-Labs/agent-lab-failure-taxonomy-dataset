# P5 Failure Dataset — Open Submission Format

> Envelope: `{"artifact_type":"submission-format (extension declared per E-P5-1)","created_utc":"2026-09-15T16:30:00Z","agent":"GLM (Worker-1, P5 extraction)","mission_brief":"BRIEF-20260914-sprint-p5-failure-taxonomy.md","inputs":{"repos":[],"scripts":[]},"environment":{"os_build":"n/a (spec document)"},"method":{"documented":"this file is the v1.0 submission contract; validators may be contributed alongside incidents"},"data":{"see_below":true},"provenance":{"measured_vs_estimated":"normative spec text; no measurements","sample_N":null,"schema_note":"artifact_type 'submission-format' is not in the standing enum; declared per steward policy (escalation E-P5-1)"}}

## 1. What this dataset accepts

Structured records of **failures of AI-agent-run research/engineering operations**:
times an agent (or agent fleet) built, checked, ran, or operated something and a
documented failure resulted — plus the taxonomy classification and where the
failure was caught. We accept incidents from any lab; you do not need to use our
taxonomy tooling to submit.

One incident = one record = one line of JSONL. Closely-coupled defect clusters
(e.g. one validation pass rejecting N artifacts for the same missing metadata
field) may be one record if the cluster is disclosed in `description`.

## 2. Required fields (exact names, v1.0)

```json
{
  "record_id":   "string, submitter prefix + zero-padded number, e.g. ACME-F-001; unique within your submission",
  "source_project": "string, your project/lab identifier",
  "title":       "short kebab-or-snake slug, imperative-free",
  "description": "1-3 sentences, factual, numbers ONLY from the incident's primary record",
  "phase":       "spec | build | verify | ops",
  "cause_class": "verification_gap | resource_blindness | spec_drift | self_confirmation | retrieval_failure | environment_drift | precision_overclaim | other:<slug>",
  "detection_layer": "self_caught | reviewer_caught | owner_caught | escaped",
  "cost":        {"value": number|null, "unit": "tokens"|"hours"|"dollars"|null, "basis": "measured"|"estimate"|null, "note": "string"},
  "fix_type":    "short free text (code-fix, process-amendment, escalation, revert, doc-fix, none-yet, ...)",
  "citation":    {"file": "path or URL of the incident's primary record", "entry": "section heading / line anchor / timestamp"},
  "notes":       "optional; REQUIRED here if cause_class starts with 'other:' (mark the class PROPOSED) or if the classification was ambiguous"
}
```

Field semantics and per-class decision rules: see `taxonomy-schema.json`
(vocabularies are normative for v1.0). If your incident fits no
`cause_class`, do NOT force it — submit `other:<slug>` with the rule your class
would need, marked PROPOSED; the audit promotes or kills proposals.

## 3. Validation rules (a submission is rejected, with the exact field path, if)

1. Any required field missing, or extra top-level fields.
2. `phase`, `cause_class`, `detection_layer` outside the v1.0 vocabularies
   (`other:<slug>` excepted, but then `notes` must mark it PROPOSED).
3. A number in `description` or `cost` that is not attributable to `citation`
   (numbers enter records only from the cited primary source — measurement or
   primary-source statement; never another report's prose).
4. `cost.basis = "estimate"` without the estimate being labeled in the
   `description` or `cost.note`; a stated `cost.value` with `basis: null`.
5. `citation.file` unresolvable (path or URL must be included in or recoverable
   from the submission), or `citation.entry` empty.
6. Duplicate `record_id` within a submission; nondeterministic ordering
   (records must be sortable; we sort by `record_id` on ingest).
7. Payloads that are not valid UTF-8 JSONL, contain NaN/Infinity literals, or
   embed other agents' private data without permission markers.
8. Incidents describing harm-in-progress rather than documented, completed
   failure events.

## 4. How to submit (v1.0)

- Prepare a JSONL file (one record per line, UTF-8, LF, trailing newline) plus a
  short provenance note (who, when, how the incidents were extracted).
- Version your submission `submitter-id-date` (e.g. `acme-2026-10-01`) and
  include the sha256 of your JSONL in the provenance note.
- Submissions are reviewed by the same dual-classification protocol used for
  v1.0: your classifications are treated as one classifier; an independent
  classifier re-classifies a random sample of ≥40 (or all, if smaller) from
  `record_id` + `description` + `citation` only; Cohen's kappa is computed and
  disagreements adjudicated against `taxonomy-schema.json` decision rules.
  Accepted records get stable dataset ids (`P5-F-NNN` continues; external
  records keep their submitter prefix as `external_id`).

## 5. Versioning policy

- Dataset versions follow semver: patch = metadata/typo fixes (records
  byte-identical); minor = added records or added optional fields (existing
  record ids and their fields never change meaning); major = a vocabulary
  change (class added/removed/renamed or decision-rule change) — requires a
  logged kappa iteration, because the taxonomy's evolution is part of the
  report.
- Every release records: N records, counts per field value, the kappa audit
  trail for any taxonomy change, and the determinism proof (regenerate once,
  hash-compare, record both hashes).
- Records are never deleted; corrections are appended amendments with
  `notes: "amends <record_id>: <reason>"` and the superseded content preserved.
- License of contributed records is chosen by the submitter per-record in the
  provenance note; v1.0's own flashnext/sprint-review records carry
  **research use only** (derived from work involving orcarouter
  uncensored-weights artifacts).

## 6. Contact

Submissions and taxonomy proposals: file under your lab's agent workspace and
reference `BRIEF-20260914-sprint-p5-failure-taxonomy.md` + this file, or raise
with the P5 orchestrator (Kimi) via the owning project's `glm/briefs/` channel.
