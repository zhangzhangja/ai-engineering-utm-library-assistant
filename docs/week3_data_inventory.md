# Week 3 Data Inventory

Project: UTM Library Services Assistant  
Generated: 2026-11-06T21:54:15.205717+00:00  
Selected pathway: Pathway A — document ingestion

## Requirement mapping

- FR1–FR2: approved undergraduate library-service content and multi-section evidence.
- FR3 and RAI1: source identity plus page/section locators.
- NFR4 and RAI4: versioned, replaceable sources with provenance and applicability controls.

## Source inventory

| Source | Role | Inspection evidence | Preparation route and status | Access limitation |
|---|---|---|---|---|
| S02 | Primary authority for edition-specific policy | 76 pages; 0 text pages; 0 characters; SHA-256 `d797976979b9cc402733e9ece2708e2c7b0dd459573e23afa171468dac091fcf` | HTML5 metadata/outline audit + scoped page OCR; K2 candidates require rendered-page review | Publicly viewable official source; redistribution permission not verified |
| S17 | Supplementary question set; policy statements require authority checking | 85 pages; 85 text pages; 120965 characters; SHA-256 `0a0e9378e71d5eaf18e1fe16569f7e6aa9a75b1b410661098c82cf970e9080f9` | Native PDF extraction + page-aware paragraph chunks; K2 supplementary candidates | Publicly viewable official source; redistribution permission not verified |

## Source precedence

S02 is the default authority for edition-specific policy. S17 and current official webpages are supplementary. S17 never overrides S02 or a newer applicable official notice; unresolved conflicts are held for review.

## Implementation decision

Both sources are implemented in one Pathway A pipeline. S02 uses a justified HTML5-assisted OCR route because native extraction failed; S17 uses its text layer. Prepared outputs remain K2 candidates and are not approved knowledge.

Raw snapshots and extracted source text remain local unless redistribution permission is confirmed.
