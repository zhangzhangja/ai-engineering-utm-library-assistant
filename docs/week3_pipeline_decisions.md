# Week 3 Pipeline Decisions

Project: UTM Library Services Assistant

## Source and requirement decisions

S02 Handbook 2026/2027 is the primary policy authority. S17 FAQ 2025 is supplementary and cannot override S02 or a newer applicable official notice. Both branches preserve page-level provenance for FR3/RAI1 and replaceable, versioned source records for NFR4/RAI4.

S02 native extraction failed (FP-01), so the pipeline uses the official HTML5 publication for identity/page structure and a frozen official-origin snapshot for scoped OCR. All S02 and S17 outputs remain K2 candidates; they are not approved answering knowledge.

## Part B comparison

| Method | Chunks | Pages | Median length | Maximum | Empty |
|---|---:|---:|---:|---:|---:|
| Fixed-size | 281 | 85 | 499 | 500 | 0 |
| Paragraph-aware | 351 | 85 | 364 | 500 | 0 |

Selected S17 configuration: `page-aware paragraph-aware character chunks; size=500; overlap=0`. It better respects observed paragraph/line boundaries while retaining page locators. It is not semantic chunking and cannot repair PDF reading-order or table errors.

## Workflow decisions

- Hybrid batch preparation: retain source snapshots, staged outputs and hashes for controlled reprocessing.
- S17 branch: native text extraction, paragraph-aware chunks and a full rerun repeatability check.
- S02 branch: HTML5 audit, selected-page OCR, quality screen, manual review queue and deterministic post-OCR preparation check.
- Current official focused webpages remain deferred supplementary sources for later conflict checking.

## Failure points and controls

- FP-01: empty native extraction routes S02 to HTML5-assisted OCR.
- FP-02: minimum OCR quality-screen failures enter the manual review queue.
- Separate validation guards enforce non-empty output, required schema, unique IDs and reproducible checksums; these guards passed in this run.

## Access and approval caveat

A hash proves byte identity, not authority, permission or policy correctness. Raw snapshots and extracted text stay local unless redistribution is permitted. Tables, amounts, eligibility, duration and campus-specific conditions require rendered-page review before promotion from K2 candidate to approved knowledge.
