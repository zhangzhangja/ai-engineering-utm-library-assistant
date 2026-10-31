# V1 Architecture Decisions and Traceability

Project: UTM Library Services Assistant  
Version: 0.2  
Updated: 1 November 2026  
Status: Conceptual design draft. Components describe required capabilities, not implemented services or selected products.

## Starting point

The simplest starting architecture is `Student question → Application → Language model capability → Response`. By itself, this does not establish access to approved UTM information, preserve citations, enforce the informational boundary or produce evaluation evidence.

The proposed [V1 diagram](week2_architecture_v1.png) adds only capabilities justified below. Its [editable Draw.io source](week2_architecture_v1.drawio) is included. No model vendor, vector database, interface framework or hosting provider is selected in this version.

## Architecture needs derived from requirements

| Requirement | What does it need? | Architecture response |
|---|---|---|
| FR1/FR2; NFR1 | Relevant undergraduate information with applicable conditions, including evidence from multiple sections | Reviewed knowledge collection and knowledge-access capability |
| FR3; RAI1 | Stable source identity and usable locators, plus a way to detect missing/invalid citations | Provenance retained through preparation, retrieval, answer composition and response checks |
| FR4 | Distinguish missing material context from an answerable question | Query context check and a clarification/conditional-response route |
| FR5; RAI2/RAI4 | Detect absent or unresolved evidence and avoid definitive unsupported answers | Evidence sufficiency/conflict status and a fallback route |
| FR6; RAI2 | An informational-only interface with honest boundaries | Scope/action check and an official-service redirect; no account-action connector |
| FR7; RAI3 | Check privacy before any external model call; avoid raw-sensitive logging | Input privacy control and minimal de-identified evaluation records |
| NFR2 | A short, measurable processing path | Prepared sources and bounded knowledge access; time the full request instead of adding an unneeded separate latency component |
| NFR3 | Explicit failure handling | Application error/timeout handling and a response route that reports failures honestly |
| NFR4; RAI4 | Reviewed source changes without retraining; retained metadata | Separate source preparation/update workflow and versioned knowledge collection |
| O3; E1–E8 | Repeatable observations for all core criteria | Evaluation point connected to case IDs, source IDs, outputs, timings and error status |

## Components and information flow

| ID | Capability | Role |
|---|---|---|
| K1 | Candidate official sources | Current handbook and relevant official pages; older sources are comparison material until explicitly approved |
| K2 | Source review and preparation | Verify completeness, version, user/campus applicability and material conflicts; normalise suitable content from approved HTML, HTML5 flipbook, PDF or another reviewed local representation while preserving the original source authority and provenance |
| K3 | Approved knowledge collection | Store reviewed passages and metadata for the frozen source snapshot |
| A1 | Application interface | Accept supported English questions and display responses; explain the informational scope |
| A2 | Query and privacy control | Check scope, material missing context and sensitive input before external processing |
| A3 | Knowledge access | Retrieve relevant approved evidence and identify insufficient or unresolved conflicting evidence |
| A4 | Language model service | Compose an understandable answer from the question and approved evidence while preserving source IDs |
| A5 | Response checks and handling | Check source IDs, references, scope/capability wording and error status; withhold unsupported output and select a fallback where required |
| A6 | Student response | Return a cited explanation, a clarification question, or a clear limitation/official redirect |
| E0 | Evaluation evidence | Capture de-identified test evidence for correctness, support, controls, latency, reliability and update tests |

The runtime path is `Student → A1 → A2 → A3 → A4 → A5 → A6`. A2 can return a clarification or scope/privacy response without answer generation. A3 can send insufficient/conflicting evidence to A5 for a fallback without invoking A4. A6 is displayed by A1 to the same student; separate diagram boxes distinguish input and response roles, not separate applications.

The source-preparation path is `K1 → K2 → K3`, and A3 reads K3. It runs outside the live question-answering path; “offline preparation” does not mean that every source must be an offline PDF. K1–K3 specify the conceptual corpus lifecycle for Lab 2, not a requirement to complete extraction, JSON/Markdown normalisation, chunking or indexing in this lab. Raw source content is not trusted as instructions to the assistant. Evaluation observes de-identified processing records without becoming a prerequisite manual step for every ordinary user response.

Simple response checks can verify citation format and source identity. They cannot guarantee semantic truth. Semantic evidence support remains an explicit human-reviewed evaluation in E2 until a justified automated method is introduced and validated.

## Architecture decision records

| Decision | Requirement / constraint | Alternatives considered | Selected option | Trade-off accepted |
|---|---|---|---|---|
| ADR1: Add reviewed knowledge access and provenance | FR1–FR3; NFR1; RAI1/RAI4; changing and overlapping official sources | Base model only; keyword search only; knowledge retrieval followed by answer composition | A reviewed knowledge collection with source/version metadata and a retrieval-and-composition path; retain keyword search as an evaluation baseline | Better traceability and a testable path to grounded explanations, at the cost of source preparation, retrieval latency and continued hallucination risk |
| ADR2: Add explicit clarification, fallback and privacy boundaries | FR4–FR7; RAI2/RAI3; no private-account access or transaction API | Answer every query; rely only on instructions inside the model; explicit controls around the model | Query/privacy checks before external processing, evidence-status routing and response checks; official redirect for actual actions | Less unsupported or misleading behaviour, but possible wrong refusals, extra interaction turns and additional processing |
| ADR3: Use a reviewed snapshot with separate update workflow | NFR4; RAI4; one-student project; inconsistent old/current pages | Live browsing of arbitrary pages on every query; a single unversioned document; reviewed versioned snapshot | Approved undergraduate-relevant source sections with recorded edition, retrieval date and applicability; controlled update and regression test | Reproducible evaluation and manageable maintenance, but no guarantee of immediate awareness of new notices or temporary changes |

These are provisional choices. A later observation may justify revising them; document the trigger, alternatives and impact on requirements/evaluation rather than silently expanding the architecture.

## Initial requirements traceability matrix

| Objective | Requirement | Evaluation criterion | Architecture element | Later evidence |
|---|---|---|---|---|
| O1/O3 | FR1/FR2; NFR1 | E1 answer correctness | K2/K3/A3/A4/A5 | Source-backed reference answers, scored A cases and failure analysis |
| O1/O2/O3 | FR3; RAI1 | E2 citation presence and support | K2/K3/A3/A4/A5 | Claim/reference audit with original section locators |
| O2/O3 | FR4 | E3 clarification | A2/A6 | C-case responses and context-handling scores |
| O2/O3 | FR5/FR6; RAI2 | E4 fallback and scope | A2/A3/A5/A6 | U/O-case responses, redirects and zero false-action-claim count |
| O2/O3 | FR7; RAI3 | E5 privacy control | A2/E0 | Synthetic P-case payload/log inspections and credential-request check |
| O3 | NFR2 | E6 latency | A1–A6/E0 | Complete request timings, configuration and error records |
| O2/O3 | NFR3 | E7 technical reliability | A1/A5/A6/E0 | Successful-response and error counts over the 50-case workload |
| O1/O2/O3 | NFR4; RAI4 | E8 update and applicability | K2/K3/A3/A5/E0 | Provenance audit, controlled-update timing and five regression results |

All “Later evidence” entries are planned artefacts. No results are claimed in Week 2.

## Review checklist

- Every major component has a corresponding requirement or evaluation purpose.
- Campus, membership category and source edition are handled through applicability, not inferred from an unlabelled passage.
- Privacy checks precede external processing; evaluation evidence avoids unnecessary sensitive data.
- Scope and insufficient-evidence routes do not depend on pretending an account action was performed.
- The design separates source preparation from answering and can be tested against keyword search.
- The diagram names capabilities rather than vendors. Transaction automation, agent memory, model training and elaborate deployment infrastructure are not justified by the current scope.

## Part C Reflection

ADR1, adding reviewed knowledge access with provenance, involved the most important trade-off. It was driven mainly by FR1–FR3, NFR1, RAI1 and RAI4, which require correct undergraduate information, evidence from relevant sections and verifiable references with the correct version and applicability. A base model or simple keyword search would be easier and faster to implement, but it would provide weaker control over grounding, source identity and multi-section answers. The selected approach accepts additional source preparation, retrieval latency, metadata management and continuing hallucination risk in exchange for better traceability, more reproducible evaluation and a practical path for withholding unsupported answers.
