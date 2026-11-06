# Week 3 Failure-Point Register

| ID | Pipeline stage | Detectable condition | Response | Observed status |
|---|---|---|---|---|
| FP-01 | INSPECT / ROUTE | Native PDF extraction returns zero usable characters or zero extractable pages. | Do not chunk empty text; route S02 to official HTML5 metadata acquisition plus page OCR. | TRIGGERED — 0 extractable pages / 0 usable characters |
| FP-02 | OCR QUALITY GATE | A selected OCR page fails the minimum text-length or alphanumeric-ratio screen. | Place affected page records in the manual review queue; keep all S02 records blocked from answering. | TRIGGERED — manual review queue pages 17, 24, 27 |
