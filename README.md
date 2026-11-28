# UTM Library Services Assistant

> A document-grounded assistant under development for UTM undergraduate students. It is designed to answer library service questions using selected official sources and traceable references.

## Problem Statement

UTM students may need several official pages to answer questions about membership, borrowing, renewal, and library rules. The proposed assistant will retrieve official evidence and answer natural-language questions with source references. Its usefulness will be evaluated against keyword search.

## Evidence of the Problem

- **Evidence 1:** UTM Library publishes [membership information](https://library.utm.my/library-membership-2-0/) and [rules and etiquette](https://library.utm.my/library-rule-and-etiquette/) on separate official pages. Relevant information is distributed across sources.
- **Evidence 2:** The [official FAQ](https://library.utm.my/libfaq/) contains questions in General, Services, Facilities, and Collection, and some answers point readers to other official resources. These questions provide an initial set of information needs. This evidence does not establish user dissatisfaction or time savings.

## Initial Related Work and Existing Approaches

1. **[UTM Library FAQs](https://library.utm.my/libfaq/):** Searchable predefined answers. Questions covering several conditions may require users to locate and combine multiple entries. This is an assessment of the FAQ structure, not measured evidence of poor user experience.
2. **[Lewis et al. (2020), Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks](https://arxiv.org/abs/2005.11401):** Provides the retrieval-plus-generation basis. Its results do not establish performance on UTM library information; local data preparation and evaluation remain necessary.
3. **[Asai et al. (2022), Evidentiality-guided Generation for Knowledge-Intensive NLP Tasks](https://aclanthology.org/2022.naacl-main.162/):** Examines whether retrieved passages contain evidence for an answer. Its training method is outside this initial prototype; the project will test evidence sufficiency and fallback behavior.
4. **[Gao et al. (2023), Enabling Large Language Models to Generate Text with Citations](https://aclanthology.org/2023.emnlp-main.398/):** Informs citation and answer evaluation. Its benchmark does not establish performance on UTM library questions; a domain-specific test set is required.

## Engineering Gap and Limitation

The practical limitation is that questions combining topics or conditions can require students to locate and combine information from several entries and pages. The project will build and evaluate an assistant that retrieves relevant evidence, gives source references, asks for clarification when needed, and avoids unsupported answers. Whether it improves on keyword search remains an evaluation question.

## Project Objectives

- **O1:** Design a document-grounded question-answering assistant for UTM undergraduate students using selected public library documents.
- **O2:** Develop retrieval-augmented generation, source references, clarification prompts, and fallback responses for unsupported questions.
- **O3:** Evaluate answer correctness, evidence support, ambiguous and unsupported questions, and response time against keyword search using a predefined test set.

## Target Users and Scope

UTM undergraduate students, especially new students seeking general library information. The assistant provides information only. It does not access student accounts, renew books, reserve materials, or process payments. Campus-specific and user-category-specific conditions must be checked against the source before answering. English is the initial test language.

## Data Sources

1. **[UTM Library FAQs](https://library.utm.my/libfaq/):** Official live FAQ. Local PDF snapshots of General, Services, Facilities, and Collection were captured on 29 October 2026.
2. **[UTM Library Membership](https://library.utm.my/library-membership-2-0/):** Student membership and account activation. Local PDF snapshot captured on 29 October 2026.
3. **[UTM Library Rules and Etiquette](https://library.utm.my/library-rule-and-etiquette/):** General library rules. Local PDF snapshot captured on 29 October 2026.
4. **[UTM Library Handbook 2026/2027](https://library.utm.my/handbook/):** [Online handbook](https://online.fliphtml5.com/rljlh/UTM-Library-Handbook-2026_digital/#p=1). Local 76-page snapshot `UTM Library Handbook 2026-2027.pdf` was exported from the official reader and visually checked on 1 November 2026; it is an image-based snapshot, not a publisher-issued download.
5. **[UTM Library FAQs 2025 Compilation](https://library.utm.my/libfaq/2025_compilation/):** [Online reader](https://heyzine.com/flip-book/2103199415.html), the latest available compilation as of 29 October 2026. Local 85-page snapshot `UTM Library FAQs 2025 Compilation.pdf` was exported from the reader and visually checked on 1 November 2026; it is not a publisher-issued download.

Printed PDF snapshots and their original webpages represent the same sources. Before using content, check relevance, duplicate entries, dates, campus, student category, and conflicting information. Do not commit copyrighted source documents to this public repository.

## Week 3 V1 Data Pipeline

Week 3 implements the first executable data pipeline for the project using **Pathway A: document ingestion**. The supplied dummy dataset was not used because the approved project sources were available.

Two document branches are implemented:

1. **S02 Handbook 2026/2027:** Primary policy authority. Native PDF extraction returned no usable text, so the pipeline records official HTML5 publication metadata and applies scoped page OCR to a frozen official-origin snapshot. Every OCR record remains a `K2_candidate` pending rendered-page review.
2. **S17 FAQ 2025 Compilation:** Supplementary source. Its text layer is prepared using page-aware paragraph chunks. It cannot override S02 or a newer applicable official notice.

The executed Colab run completed without errors and produced:

- 351 unique S17 prepared records;
- 67 S02 OCR candidate records from 22 selected pages;
- a five-record manual review queue covering pages 17, 24 and 27;
- branch manifests, a combined source-priority manifest and explicit FP-01/FP-02 evidence;
- a full-rerun repeatability pass for S17 and a deterministic post-OCR preparation pass for S02.

### Week 3 repository artefacts

- [Executed pipeline notebook](notebooks/week3_pipeline.ipynb)
- [Data inventory](docs/week3_data_inventory.md)
- [Pipeline decisions](docs/week3_pipeline_decisions.md)
- [Failure-point register](docs/week3_failure_register.md)
- [Pipeline diagram](docs/week3_pipeline_diagram.png)
- [Part B lineage manifest](data/week3_manifest.json)
- [Combined branch manifest](data/week3_combined_manifest.json)
- [S17 execution manifest](data/pipeline_v1/documents/pipeline_manifest.json)
- [S02 scope, HTML5 audit and execution manifest](data/pipeline_v1/handbook_html5_ocr/)
- [Data storage and publication note](data/README.md)

The raw PDFs, OCR text, prepared JSONL files and manual-review records remain outside the public repository because redistribution permission has not been verified. Their filenames, hashes, counts and processing relationships remain documented in the committed manifests.

The Week 2 and Week 3 diagrams use the same question-and-answer flow (A1-A6 and E0). Week 3 expands K1-K2 within DATA into the two executed document branches, six pipeline stages, failure responses and lineage outputs. Candidate review remains separate from K3 approved knowledge. The notebook regenerates the diagram in its diagram cell and refreshes run-specific observations from the preceding cells.

### Reproducing the Colab run

1. Open `notebooks/week3_pipeline.ipynb` in Google Colab.
2. One-time setup: place `UTM Library Handbook 2026-2027.pdf` and `UTM Library FAQs 2025 Compilation.pdf` in your Google Drive folder `MyDrive/AE_Lab3/inputs/`. Cell 2 mounts Drive and copies them into the workspace automatically (it falls back to manual upload only if that folder is empty).
3. Run all cells from top to bottom.
4. Confirm `FINAL CHECKPOINT PASS` in the final validation cell.
5. Cell 26 saves the artefact ZIP to `MyDrive/AE_Lab3/outputs/`, overwriting the previous one each run. Keep it private unless redistribution permission is confirmed.

## Week 4 Data Quality and Responsible Data Review

Week 4 validates, reviews, governs and remediates the Topic 3 prepared corpus before any retrieval
work. No model or embedding call is made. The Topic 3 per-source files (S17 FAQ and S02 handbook) are
combined (418 chunks) and reduced to a reviewed corpus of 373 chunks (S17 311, S02 62).

Remediation applied (all recorded, none silent):

- S17 re-chunked with overlap (paragraph/sentence aware, size 500, overlap 80): broken fragments such
  as `ties.` or `Lumpur` are gone; 20 fragments became 11 complete headings.
- 7 S17 FAQ navigation pages (cover + section dividers) and 5 S02 chunks from 3 divider photo pages excluded and
  logged ([excluded-records log](docs/week4_excluded_records.csv)); divider titles kept as
  `faq_section` / `handbook_section` topic tags.
- 4 S17 lost `U+FFFD` symbols stripped; the S17 page-32 duplicate removed during reconstruction.
- Source precedence recorded in metadata: every chunk carries `authority_rank` (S02 = 1 primary,
  S17 = 2 supplementary).
- 11 broken-word spacing splits in S17 (`Ema il`, `Ma terials`) dictionary-merged and logged;
  near-duplicate and cross-source overlap checked lexically (4 within-source pairs flagged, 0
  cross-source). Keyword checks identify membership, borrowing/renewal and fines/rules terms in
  the sampled S02 pages; full policy and exception coverage still requires review. The listed K3
  facts are checked against rendered S02 pages. The student membership table on PDF system p18
  states "Refer university fees rate"; a payable fee amount is not established. The six replacement
  records and fines cite p28; the loan table cites p25. The RM50 fee on p21 is specific to Corporate
  membership (within the country) and is excluded from undergraduate V1 retrieval.

Headline findings (full evidence in the report):

- Completeness and traceability pass: 0 of 373 chunks miss text, source id, URL, page or version.
  Nothing is answerable before review (`approved_for_answering = False` for all K2 candidates).
- Extraction noise resolved: replacement-character and broken-fragment issues cleared; the
  OCR-garbled handbook pages were confirmed to be decorative dividers, not lost policy text.
- Representation signal: the primary authority S02 is 16.6 percent of chunks (19 policy pages) versus
  supplementary S17 at 83.4 percent, driven by the handbook being an image-only, photo-heavy source.
- S02 300 DPI re-OCR was tested and held: the local OCR engine misread text and tables stayed
  jumbled, so it is deferred to Colab (tesseract 5.3.4) plus human table review rather than degrading
  the primary source.
- Quality gate: **GO WITH CONDITIONS**. The knowledge file contains 20 in-scope K3 records and
  one excluded corporate transfer record. All 373 K2 chunks remain unapproved. The listed K3 facts
  are checked; other OCR prose and university fee amounts are not established. Remaining work
  includes policy coverage, source-text redistribution rights and implementing retrieval controls
  for approval, applicability and `authority_rank`.


### Week 4 repository artefacts

- [Quality notebook](notebooks/week4_quality.ipynb)
- [Data quality, bias and responsible data report](docs/week4_data_quality_report.md)
- [Validation failure register](docs/week4_validation_failures.csv)
- [Duplicate review register](docs/week4_duplicate_review.csv)
- [Excluded-records log](docs/week4_excluded_records.csv)
- [De-spacing log](docs/week4_s17_despacing.csv)
- [Near-duplicate review](docs/week4_near_duplicate_review.csv)
- [Source rights-verification record](docs/week4_rights_verification.md)
- [Fee/fines/loan cross-check](docs/week4_fee_fines_crosscheck.md) + [verified table](docs/week4_verified_fees.csv)
- [Approved policy tables for Topic 7](data/approved_policy_tables.json) (K3, structured, with page provenance)
- [Coverage chart](docs/week4_coverage_by_source.png)

The reviewed corpus (`data/prepared_chunks_clean.jsonl` and `data/chunks_clean.pkl`) is the Topic 4
output that later stages must use. From **Topic 7 (Week 9)** onward, embedding, indexing and retrieval
use the reviewed Topic 4 corpus for preparation. Answering must use explicitly approved records and
honour applicability and `authority_rank`; the 373 K2 chunks remain blocked until individual approval. For fee, fines, loan and replacement values, Topic 7 should retrieve the verified records in
`data/approved_policy_tables.json` (K3-approved, with source and page provenance) instead of the raw
S02 OCR text. These corpus files stay local because source-text redistribution permission is not verified;
only public-safe evidence is committed.

## Google Drive layout for Colab

The Colab notebooks read inputs from and write outputs to a fixed per-lab Google Drive layout, so you
set a folder up once and do not upload files by hand each session.

Convention: `MyDrive/AE_Labs/AE_Lab<N>/inputs` and `MyDrive/AE_Labs/AE_Lab<N>/outputs`.

| Lab | `inputs/` (you place these) | `outputs/` (the notebook writes these) |
|---|---|---|
| AE_Lab3 | the two source PDFs (handbook + FAQ) | the Week 3 artefact ZIP |
| AE_Lab4 | `prepared_text.jsonl` and `prepared_handbook_text.jsonl` (Week 3 outputs) | reviewed corpus plus the week4 evidence files |

How the notebooks use it: each notebook mounts Drive (authorise once per session), then reads from
`INPUT_DIR` (`.../inputs`) and saves to `OUTPUT_DIR` (`.../outputs`). If Drive is not mounted it falls
back to `/content` (manual upload) and does not crash.

Governance: the `outputs/` folders can contain extracted source text, so keep them private in your own
Drive. Only metadata and evidence files are committed to this repository.

## Responsible AI Risks

- **Incorrect or outdated information:** Cite sources, record source dates, and flag missing or conflicting evidence.
- **Privacy:** Do not request credentials; minimise stored query data and remove personal information from evaluation records.
- **Misleading capability claims:** Explain that the assistant provides instructions only and direct users to official services for account actions.

## Weekly Progress

| Week | Focus | Deliverable | Status |
|---|---|---|---|
| W1 | System analysis and project scoping | [Observation sheet](docs/week1_observation_sheet.md), [system map](docs/week1_system_map.png), [project scope](docs/week1_project_scope.md), README | Updated on GitHub; course portal PDF submission handled separately |
| W2 | Requirements, evaluation planning and V1 architecture | [Requirements](docs/week2_requirements.md), [evaluation criteria](docs/week2_evaluation_criteria.md), [architecture diagram](docs/week2_architecture_v1.png), [architecture decisions](docs/week2_architecture_decisions.md), [source register](docs/week2_source_register.md) | Completed; source PDFs remain local pending redistribution permission |
| W3 | Data ingestion, preparation, lineage and executable V1 pipeline | [Notebook](notebooks/week3_pipeline.ipynb), [inventory](docs/week3_data_inventory.md), [diagram](docs/week3_pipeline_diagram.png), [decisions](docs/week3_pipeline_decisions.md), [manifests](data/) | Completed; public-safe evidence committed, source and prepared text retained locally |
| W4 | Data quality, bias and responsible-data review | [Quality notebook](notebooks/week4_quality.ipynb), [report](docs/week4_data_quality_report.md), [validation register](docs/week4_validation_failures.csv), [duplicate register](docs/week4_duplicate_review.csv), [excluded-records log](docs/week4_excluded_records.csv), [de-spacing log](docs/week4_s17_despacing.csv), [near-duplicate review](docs/week4_near_duplicate_review.csv), [rights record](docs/week4_rights_verification.md), [coverage chart](docs/week4_coverage_by_source.png) | Completed; public-safe evidence committed, reviewed corpus retained locally |

