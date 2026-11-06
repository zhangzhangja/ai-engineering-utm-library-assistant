# Week 3 Data Publication Note

This directory contains public-safe lineage and execution metadata for the Week 3 V1 document pipeline.

## Committed evidence

- `week3_manifest.json`: Part B lineage for the S17 FAQ preparation.
- `week3_combined_manifest.json`: Combined S02/S17 authority and branch mapping.
- `pipeline_v1/documents/pipeline_manifest.json`: S17 Part C execution manifest.
- `pipeline_v1/handbook_html5_ocr/handbook_v1_scope.json`: Selected S02 page scope and requirement mapping.
- `pipeline_v1/handbook_html5_ocr/html5_acquisition_audit.json`: Official HTML5 publication audit.
- `pipeline_v1/handbook_html5_ocr/pipeline_manifest.json`: S02 OCR-branch processing and validation metadata.

## Retained locally

The following generated or source-content files are intentionally excluded from the public repository:

- `data/raw/*.pdf`
- `data/processed/prepared_text.jsonl`
- `data/pipeline_v1/documents/prepared_text.jsonl`
- `data/pipeline_v1/handbook_html5_ocr/interim/handbook_ocr_raw.jsonl`
- `data/pipeline_v1/handbook_html5_ocr/prepared_handbook_text.jsonl`
- `data/pipeline_v1/handbook_html5_ocr/handbook_manual_review_queue.jsonl`

They are stored in the private Week 3 artefact package downloaded from Colab. Redistribution permission has not been verified. The committed manifests retain their locations, hashes, counts and processing relationships without publishing the extracted source text.
