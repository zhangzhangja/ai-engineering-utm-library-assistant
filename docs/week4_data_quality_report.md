# Data Quality, Bias, and Responsible Data Report - UTM Library Services Assistant

Project: UTM Library Services Assistant (individual semester prototype)
Phase: Phase 1, Topic 4 (data quality and responsible data practices)
Prepared from: the Topic 3 hand-off (S17 FAQ 2025 + S02 Handbook 2026/2027)
Corpus under review: 418 input chunks, 373 after cleaning, re-chunking, de-spacing and exclusions
Status: evidence for the Week 7 Proposal and the Week 14 Responsible AI review

This report separates measured findings, engineering judgement, unresolved uncertainty, planned
mitigation and residual risk. Ratings use a 1 to 5 scale, or N.V. (Not Verified) where evidence is
not yet available. Embeddings, retrieval and generation are out of scope for Topic 4.

Remediation applied in this review (all recorded, none silent):
- S17 fragments: the source was re-chunked with overlap (paragraph and sentence aware, size 500,
  overlap 80, trailing-merge). Broken tails such as `ties.` or `Lumpur` are gone; 20 fragments became
  11 complete short headings.
- S17 navigation: 7 FAQ navigation pages (cover + 6 section dividers) were dropped as non-content and
  logged (`docs/week4_excluded_records.csv`).
- S02 dividers: 5 chunks (pages 17, 24, 27) excluded as confirmed section-divider photo pages, logged.
- Lost symbol: 4 S17 `U+FFFD` map-pin symbols stripped; text and URL kept.
- Duplicate: the S17 page-32 repeat was removed during page reconstruction.
- Topic tags added: `faq_section` (S17) and `handbook_section` (S02).
- Source precedence made operational: `authority_rank` (S02 = 1 primary, S17 = 2 supplementary) and
  `authority_tier` added to every chunk.
- De-spacing: 11 high-confidence broken-word splits in S17 (for example `Ema il`, `Ma terials`) were
  dictionary-merged and logged (`docs/week4_s17_despacing.csv`).
- Near-duplicate and cross-source overlap were checked lexically; results logged
  (`docs/week4_near_duplicate_review.csv`).

---

## 1. Data Quality Scorecard

| Dimension | Rating (1-5 / N.V.) | Evidence | Action / limitation |
|---|---:|---|---|
| Completeness | 4 | 0 of 373 chunks miss `text`, `source_id`, `source_url`, `page` or `version` (rules R1-R4 pass). Topic tags now present (`faq_section`, `handbook_section`). S02 policy text covers 19 handbook pages. | Field and topic completeness accepted; confirm no supported-task policy is missing from S02. |
| Consistency | 5 | One uniform schema; one overlap-based chunking scheme after re-chunk; source precedence encoded as `authority_rank`; 0 exact duplicates; no whitespace noise. | 11 high-confidence spacing splits in S17 were dictionary-merged and logged; a few acronym splits (for example `U TMKL`) are left untouched as low-risk. |
| Accuracy | 4 | `U+FFFD` stripped (R5 = 0) and the 5 OCR-garbled divider pages excluded. The critical S02 tables (loan periods, fines, replacement charges, registration) were verified against the rendered handbook pages (`docs/week4_fee_fines_crosscheck.md`), resolving the S09-vs-S16 fine conflict; no open table flags remain. Non-table S02 prose is still OCR and unverified. | Verify remaining non-table S02 prose if used for answering. |
| Timeliness | 3 | Every chunk carries `version` and `capture_date` (S17 = 2025; S02 = 2026/2027). Applicability to the current academic year is not machine-verifiable. | Keep source-precedence and timeliness metadata; resolve applicability by human review. |
| Uniqueness | 5 | 0 exact duplicates after page reconstruction (the earlier S17 page-32 repeat is resolved). Near-duplicate and cross-source overlap not yet checked. | Add near-duplicate and S17/S02 overlap review later. |

---

## 2. Validation and Data-Defect Findings

Seven repeatable rules were run with Pandera and a governance-safe register
(`docs/week4_validation_failures.csv`).

| Rule | Condition | Failure detected | Flagged | Decision |
|---|---|---|---:|---|
| R1 text_present | `char_count > 0` | Empty chunk (FR1/FR2) | 0 | Hard fail |
| R2 source_present | `source_id` not empty | Untraceable evidence (FR3/RAI1) | 0 | Hard fail |
| R3 source_url_present | `source_url` not empty | No provenance link (RAI4) | 0 | Hard fail |
| R4 page_locator_present | valid `page` | No page/section locator (FR3) | 0 | Hard fail |
| R5 no_replacement_char | no `U+FFFD` / `U+0000` | Extraction/encoding corruption | 0 (4 fixed) | Resolved |
| R6 min_useful_length | `char_count >= 50` | Fragment too short to carry a policy statement | 11 | Review (complete headings, not broken tails) |
| R7 answering_gate | `approved_for_answering == False` for every K2 | A candidate answerable before review (RAI1/RAI4) | 0 | Hard fail - control confirmed |

R6 interpretation: after re-chunking, the 11 remaining short chunks are complete section or page
titles (for example `UTM LIBRARY BORROWING`, `UTM-IR / UTMIR`, `ISO 9001: 2015`), not mid-sentence
tails. Each is also carried into the following chunk through the chunk overlap, so no heading is
orphaned from its content. They are valid short units, kept as retrieval anchors.

Missing, corrupted, noisy, inconsistent, stale and uncertain summary: missing fields = 0;
corrupted/encoding = 0 (fixed); OCR divider noise = 0 remaining (excluded); broken fragments = 0
(re-chunked; 11 complete headings remain); exact duplicate = 0; stale/applicability = unresolved
(S17 2025 vs S02 2026/2027), held for review.

---

## 3. Duplicate and Anomaly Findings

Exact deduplication used SHA-256 over whitespace-normalised text with provenance preserved
(`docs/week4_duplicate_review.csv`).

- Input 418 chunks. After dropping 7 S17 navigation pages and 5 S02 divider pages, and re-chunking
  S17 with overlap: 373 chunks (S17 311, S02 62). Exact duplicates after reconstruction: 0.
- The earlier anomaly (S17 page-32 repeated renewal statement) is resolved: duplicate paragraphs are
  removed during page reconstruction before re-chunking.
- The S02 OCR anomaly (5 garbled chunks) is resolved: confirmed decorative dividers, excluded with
  evidence (`docs/week4_excluded_records.csv`).

Near-duplicate and cross-source overlap were also checked lexically (Jaccard on 5-word shingles,
since embeddings are deferred to Topic 7). Result: 4 within-source S17 near-duplicate pairs (top
Jaccard 0.65), all repeated FAQ instructions across different pages (for example log-out reminders on
pages 55, 57, 58); they are flagged for review, not removed, because each sits in its own page
context. Cross-source S17-S02 overlap above threshold: 0, which is expected because the FAQ and the
handbook use different wording; true semantic overlap needs embeddings and is a Topic 7/8 check.

Limitation: lexical methods catch near-identical text, not paraphrase. The corpus is not claimed to
be fully deduplicated.

---

## 4. Representation, Coverage, and Bias Assessment

Coverage is judged against application scope, not equal counts (`docs/week4_coverage_by_source.png`).

- S17 (supplementary FAQ): 311 chunks, 83.4 percent, tagged into 6 FAQ sections (GENERAL 70,
  SERVICES 53, FACILITIES 34, COLLECTIONS 57, ACQUISITIONS 29, OTHERS 68).
- S02 (primary authority handbook): 62 chunks, 16.6 percent, across 19 policy pages, tagged into
  handbook sections (MEMBERSHIPS, LOAN_ELIGIBILITY, FINES and general/services).
- Grouping is actual metadata (`source_id`, `page`, `faq_section`, `handbook_section`), not a
  proxy. No demographic or sensitive attribute was created.

**Why is S02 only about 16 percent?** Three combined reasons, not low policy importance:
1. S02 native extraction failed (FP-01), so only a scoped set of policy pages was OCR-captured; the
   handbook is an image-only flipbook, which is costly to extract.
2. The handbook is photo-heavy: cover, section-divider and photo pages carry no policy text, so the
   "76 pages" figure overstates the policy content. After excluding 3 divider pages, 19 policy pages
   remain.
3. S17 is a large 85-page FAQ with a native text layer, so it naturally yields many more chunks.
So the share reflects extraction difficulty and document shape, not that the handbook matters less.
The handbook remains the primary authority; the share is a representation signal to act on, not proof
of inadequacy.

**S02 supported-task coverage, confirmed.** A keyword scan of the kept S02 pages shows every
supported task is represented in the primary authority: membership (pages 15-23), borrowing and
renewal (pages 15-23, 28, 29, 35, 49, 51) and fines and rules (pages 15-23, 28, 29, 35, 52). The
thinnest area is explicit loan-eligibility (3 chunks), so that section should be checked first during
the human table review.

At least two evidence-based bias risks, with propagation path:

1. **Representation bias.** Evidence: primary authority S02 is 16.6 percent of chunks versus
   supplementary S17 at 83.4 percent. Origin: engineering (FP-01 extraction failure plus a photo-heavy
   handbook), amplified by raw counts. Propagation: in Topic 7 a count-blind retriever may rank
   supplementary FAQ above the authoritative handbook. Intervention: the new `authority_rank` field
   lets retrieval prefer S02 on ties or conflicts; confirm every supported-task policy is captured
   from S02. System test in Topic 8: primary-versus-supplementary retrieval hit-rate on the 30
   answerable queries.
2. **Measurement bias.** Evidence: the handbook is image-only, so all S02 text is OCR; the garbled
   divider pages are excluded, but tables and numbers on the 19 kept pages are not yet verified.
   Origin: measurement, OCR approximates the rendered page. Propagation: wrong facts (hours, fines)
   if an unverified OCR table were used. Intervention: fee, fines and loan
   tables were cross-checked against the official HTML pages (`docs/week4_fee_fines_crosscheck.md`),
   verifying most values and resolving the S09-vs-S16 journal-fine conflict (RM0.50/day by authority),
   with only 3 narrow items left for the rendered-page check; the answering gate keeps S02 blocked
   meanwhile. System test in Topic 8: OCR text versus rendered page.

A third, historical risk is recorded: S17 is the 2025 compilation while S02 is 2026/2027, so older
supplementary content could carry superseded policy. Intervention: source precedence (`authority_rank`)
plus timeliness metadata; hold unresolved conflicts with a limitation or redirect response.

This section does not claim final system fairness. Unequal counts are a signal; fairness is evaluated
at system level in Topic 8 (Week 10).

---

## 5. Privacy, Consent, Rights/Access, and Data Governance

Based on `docs/week3_data_inventory.md` and the project scope.

| Governance question | Evidence | Status | Required action / owner |
|---|---|---|---|
| Source owner, version/date, provenance traceable? | `source_id`, `source_url`, `source_sha256`, `page`, `version`, `capture_date` on every chunk | OK | Keep provenance on all later artefacts |
| Rights/access terms known and appropriate? | Both sources publicly viewable; redistribution not verified | To verify | Confirm reuse basis before any public release of source text |
| Personal or sensitive data unnecessary for the task? | Public policy text only; no personal records | No | None; do not add demographic attributes |
| Consent / collection basis appropriate? | Public institutional documents; no personal data collected | N/A | Record as N/A with reason |
| Update, superseded-version, retention, removal defined? | Source-precedence rule plus versioned sources (NFR4/RAI4) | OK (partial) | Assign a named update owner; re-capture when editions change |

Governance boundary: publicly accessible is not public domain. The S17 and S02 snapshots and all
extracted text stay local; the rights basis is marked to verify rather than assumed. Only public-safe
evidence is committed to the repository. A per-source rights-verification record with the exact action
required is in `docs/week4_rights_verification.md`.

---

## 6. Mitigation Plan and Residual Risk

| Issue | Evidence | Action | Owner / timing | Residual risk |
|---|---|---|---|---|
| S17 broken fragments | 20 chunks under 50 chars | Re-chunked with overlap; trailing-merge | Done (this review) | Resolved; 11 complete headings remain as anchors |
| S17 navigation noise | 7 cover/divider pages | Dropped and logged; section captured as `faq_section` | Done | Resolved |
| S02 OCR divider noise | 5 garbled chunks, pages 17/24/27 | Confirmed non-text dividers; excluded and logged | Done | Resolved |
| Lost symbol / duplicate | 4 `U+FFFD`; 1 page-32 repeat | Symbol stripped; duplicate removed in reconstruction | Done | Resolved |
| S17 broken-word spacing | 11 splits (`Ema il`, `Ma terials`) | Dictionary de-spaced and logged | Done (this review) | Resolved |
| Near-duplicate FAQ instructions | 4 within-source pairs | Flagged for review, kept in page context | Done (flagged) | Low |
| Source precedence not operational | S02 primary but count-dominated by S17 | Added `authority_rank` / `authority_tier` to every chunk | Done | Retrieval must use it in Topic 7 |
| S02 table/number accuracy | Image-only source; OCR on 19 pages | Fee/fines/loan/replacement/registration tables verified against the rendered handbook pages; S09-vs-S16 conflict resolved | Done (rendered-page review) | Resolved for tables; non-table prose still OCR |
| S02 re-OCR at 300 DPI | Local test: tesseract 4.1.1 misread `Users` as `OTT`; tables still jumbled | Held: not applied here to avoid degrading the primary source with an older engine | Student, in Colab with tesseract 5.3.4 | Low; optional quality gain |
| S17 native-text spacing | `facul ties`, `U TMKL` artefacts | Note for a later despacing pass | Student, Topic 7 prep | Low |
| Source-text redistribution rights | Both sources not verified | Confirm reuse basis; keep text local | Student and lecturer | Medium until verified |
| Stale supplementary policy | S17 2025 vs S02 2026/2027 | Source precedence; hold conflicts; limitation/redirect | Student, Topic 7 to 8 | Medium; needs system test |

---

## 7. Impact on Topic 2 Requirements / Evaluation Criteria

- **FR3 / RAI1 (verifiable reference).** Supported: every chunk has source id, URL and page (R2-R4
  pass). Topic tags improve locator quality. The verified fee/fines/loan values are published as
  K3-approved retrieval records in `data/approved_policy_tables.json`, each citing source and page,
  so Topic 7 can answer policy figures with provenance instead of unreliable OCR.
- **RAI4 (outdated information and applicability).** Partly supported: version and capture_date
  present; the answering gate (R7) blocks all K2 candidates; `authority_rank` encodes precedence.
  Risk if unresolved: an S17 2025 statement could be returned as current, failing E8/RAI4.
- **FR1/FR2 and NFR1 (correctness).** Risk reduced by `authority_rank` and topic tags, but S02 table
  verification is still required; otherwise the assistant may answer from supplementary FAQ.
- **RAI2 (scope and fallback).** Unaffected at data level; gaps and conflicts are recorded, not removed.

---

## 8. Topic 4 Quality Gate Decision

**GO WITH CONDITIONS.**

The corpus is complete, fully traceable and gated (R1-R4, R7 pass). The data-level defects found in
this review are resolved: broken S17 fragments were re-chunked with overlap, navigation and divider
pages excluded with evidence, symbols and the duplicate removed, topic tags added, and source
precedence made operational through `authority_rank`. No HOLD-level defect was found.

Conditions to clear before Topic 7 relies on the corpus (the one remaining step is human):

1. Done: the fee, fines, loan, replacement and registration tables are verified against the rendered
   handbook pages (`docs/week4_fee_fines_crosscheck.md`); the S09-vs-S16 fine conflict is resolved and
   no open table flags remain. Verify any non-table S02 prose only if it is used for answering.
2. Confirm every supported-task policy (membership, circulation, general rules) is captured from S02,
   and make Topic 7 retrieval honour `authority_rank`.
3. Verify source-text redistribution rights; keep all extracted text local until then.
4. Optional: re-OCR the kept S02 pages in Colab with tesseract 5.3.4; run a despacing pass on S17.

S02 answering stays blocked until table/number verification. S17 remains supplementary and cannot
override S02 or a newer official notice.

---

### Artefact index

| Artefact | File | Committed |
|---|---|---|
| Quality notebook | `notebooks/week4_quality.ipynb` | Yes (no source-text outputs) |
| Validation failure register | `docs/week4_validation_failures.csv` | Yes |
| Duplicate review register | `docs/week4_duplicate_review.csv` | Yes |
| Excluded-records log (S02 dividers + S17 navigation) | `docs/week4_excluded_records.csv` | Yes |
| De-spacing log | `docs/week4_s17_despacing.csv` | Yes |
| Near-duplicate review | `docs/week4_near_duplicate_review.csv` | Yes |
| Source rights-verification record | `docs/week4_rights_verification.md` | Yes |
| Fee/fines/loan cross-check | `docs/week4_fee_fines_crosscheck.md` | Yes |
| Verified fee/fines table | `docs/week4_verified_fees.csv` | Yes |
| Approved policy tables (K3, for Topic 7) | `data/approved_policy_tables.json` | Yes |
| Coverage chart | `docs/week4_coverage_by_source.png` | Yes |
| This report | `docs/week4_data_quality_report.md` | Yes |
| Reviewed cleaned corpus | `data/prepared_chunks_clean.jsonl`, `data/chunks_clean.pkl` | No, kept local (rights to verify) |

N.V. means Not Verified: used where accuracy, rights or coverage cannot yet be demonstrated, in
preference to inventing a rating.
