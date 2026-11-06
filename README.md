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

### Reproducing the Colab run

1. Open `notebooks/week3_pipeline.ipynb` in Google Colab.
2. Upload `UTM Library Handbook 2026-2027.pdf` and `UTM Library FAQs 2025 Compilation.pdf` when prompted.
3. Run all cells from top to bottom.
4. Confirm `FINAL CHECKPOINT PASS` in the final validation cell.
5. Keep the downloaded artefact ZIP private unless redistribution permission is confirmed.

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

