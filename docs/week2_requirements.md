# Requirements Specification

Project: UTM Library Services Assistant  
Version: 0.2  
Updated: 1 November 2026  
Status: Planning draft. Proposed requirements and acceptance targets; implementation and measured results are pending.

## Section 0 Project Framing and Application Scope

| Framing element | Project specification |
|---|---|
| Problem statement | UTM undergraduate students may need to consult several official pages or handbook sections to understand library membership, borrowing, renewal and rules. Questions involving several conditions require the student to locate the relevant information and connect it correctly. The project will investigate whether a document-grounded conversational assistant can make this process more useful while keeping answers verifiable. |
| Problem evidence | The official [FAQ](https://library.utm.my/libfaq/), [membership page](https://library.utm.my/library-membership-2-0/) and [circulation page](https://library.utm.my/borrowing-returning-reserving-renewing/) distribute related information across different locations. The official [handbook index](https://library.utm.my/handbook/) also presents different academic-year editions. This demonstrates distributed information and the need for version selection; it does not establish dissatisfaction, time savings or a measured reduction in staff workload. |
| Existing approach 1 | The [UTM Library FAQ](https://library.utm.my/libfaq/) provides predefined answers and FAQ search. It is an existing authoritative entry point. A user with a question spanning topics may still need to select and combine relevant entries. This is an initial interface/content assessment, not a completed usability study. |
| Existing approach 2 | [Lewis et al. (2020), Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks](https://arxiv.org/abs/2005.11401) combines retrieval and generation. Its reported results do not establish performance on UTM library data; source preparation and local evaluation remain necessary. |
| Existing approach 3 | [Asai et al. (2022), Evidentiality-guided Generation for Knowledge-Intensive NLP Tasks](https://arxiv.org/abs/2112.08688) considers whether retrieved passages contain evidence supporting the output. Reproducing its training method is beyond the initial prototype; the relevant principle is to evaluate actual evidence support rather than citation presence alone. |
| Engineering gap | A small application is needed to connect undergraduate library questions to approved evidence, explain applicable conditions, expose sources, clarify missing context and acknowledge insufficient or conflicting information. Whether this provides benefits over keyword search remains an evaluation question. No scientific-novelty claim is made. |
| Objective O1 | To design a document-grounded question-answering assistant for UTM undergraduate students using selected public library documents. |
| Objective O2 | To develop the assistant with knowledge retrieval, source references, clarification and appropriate fallback/scope controls. |
| Objective O3 | To evaluate correctness, evidence support, handling of ambiguous and unsupported queries, and response time on a predefined test set, using keyword search as a baseline where the outputs can be compared fairly. |
| Intended users | UTM undergraduate students, particularly students unfamiliar with library services. Postgraduate, staff, alumni and external-member entitlements are outside the initial supported user scope. |
| Supported tasks | Explain approved information about student membership activation; borrowing, returning, renewal and reservation procedures; undergraduate loan conditions; and general library rules. General published fines may be explained only when the selected sources are consistent and approved. |
| Knowledge domain | Selected undergraduate-relevant sections of official UTM Library sources. See the [source register](week2_source_register.md). The latest officially linked handbook is the primary authority for edition-specific policy, while current official webpages provide reliable supplementary information. Source representations may include HTML, an online HTML5 flipbook, PDF or a reviewed local extraction. The AY2024/2025 PDF is historical/reference material pending current-policy corroboration. |
| Campus coverage | Proposed: support information whose applicability to Johor Bahru, Kuala Lumpur or Pagoh is explicitly established by the approved sources. Do not assume all rules are campus-invariant. Ask which campus is relevant when it changes the answer; do not demand a campus when the approved rule applies across campuses. |
| Language | English is the V1 supported and formally evaluated query/answer language. Bahasa Melayu and other languages are possible later extensions, without a V1 performance claim. |
| Operating context | An individual semester prototype providing general information through a question-answer interface. It uses a reviewed source snapshot, without access to a user's private library account or live transaction state. |
| Constraints | One student and semester timescale; public-source access; source version and extraction quality; limited model/compute budget; no authorised library transaction API; no requirement to train a new foundation model. Some official material is available only as webpages or an online HTML5 flipbook without a publisher-provided download. The system is therefore source-format agnostic, but the selected content must be reviewed and captured reproducibly before final acceptance testing. Models and tools will be selected later. |
| Explicit out-of-scope tasks | Logging into accounts; checking personal loans, balances or live item availability; executing renewals, reservations or payments; deciding exemptions or waivers; general academic/medical/legal counselling; guaranteeing current opening hours or temporary operational status from a static snapshot. |
| Evaluation focus | Can the system provide correct and source-supported answers to covered undergraduate questions while correctly clarifying or declining the rest, within an acceptable response time? What evidence-retrieval benefits and processing costs are observed against keyword search using the same sources and queries? |

The library offers real renewal, reservation and payment services. Their existence does not imply that this assistant performs those actions. The assistant can explain approved procedures and provide official links; students perform authenticated actions themselves in the official service.

In this project, **document-grounded** means grounded in reviewed official content with retained provenance. It does not require every original source to be a publisher-issued PDF: an official webpage, an officially linked HTML5 handbook, a local PDF snapshot or a reviewed local representation may supply the content, provided that the original authority, URL, edition or retrieval date, and page/section locator remain traceable.

## Section 1 Functional Requirements

| ID | Requirement: The system SHALL | Scope boundary: SHALL NOT, where relevant |
|---|---|---|
| FR1 | Answer supported English questions about the defined undergraduate library-service topics using approved official information and state material user/category/campus conditions. | Invent policies, extend a policy to another user group or campus without support, or present archived information as current. |
| FR2 | Combine relevant evidence from more than one approved section when required to answer a supported question, preserving conditions and exceptions. | Treat partially retrieved evidence as sufficient for an unconditional answer. |
| FR3 | Provide a verifiable reference for each substantive document-grounded answer, including the source title, original URL and a page or section locator; disclose the edition/snapshot used where material. | Manufacture citations or imply that displaying a URL alone proves an answer is correct. |
| FR4 | Ask a concise clarification question, or give an explicitly conditional answer, when missing information could change the correct response. | Request passwords, full student IDs or private account records to clarify an informational question. |
| FR5 | State when available approved evidence is missing, incomplete or conflicting, and provide a relevant official route for further help. | Silently choose a conflicting policy or fabricate an answer to avoid a fallback. |
| FR6 | Explain that it provides information only when asked to execute an account action, and direct the user to the appropriate official service. Decline other questions outside its supported scope with a brief explanation. | Access accounts, renew or reserve items, accept payment, decide waivers, or state that such an action has been completed. |
| FR7 | Apply scope and privacy checks before external processing and retain only the minimum de-identified evidence needed for evaluation. | Request credentials or retain/transmit unnecessary direct identifiers in raw queries, model payloads or evaluation logs. |

## Section 2 Non-Functional Requirements

These are provisional engineering targets, not lecturer-mandated figures or observed results. Keep target changes versioned and justified; do not revise a target silently after seeing final-test results.

| ID | Quality attribute | Requirement | Measurement method | Metric | Initial threshold |
|---|---|---|---|---|---|
| NFR1 | Correctness | Supported answers shall be sufficiently correct for routine informational use. | Evaluate 30 predefined answerable queries against reviewed source-backed reference answers and a binary rubric; count wrong refusals and incomplete required conditions as failures. | Correct response rate: correct answerable cases / 30. | At least 85%, meaning at least 26/30 in this initial test design. |
| NFR2 | Performance | The system shall return complete responses within the interactive latency target. | Time query submission to complete displayed response for the same 30 answerable queries, sequentially in a documented environment with prepared data and an empty response cache. Include retrieval, model and validation time; record errors/timeouts separately and assign them infinite latency for the percentile. | Nearest-rank p90 end-to-end latency. | At most 8 seconds. |
| NFR3 | Reliability | The application shall handle the planned workload without unhandled technical failures. | Run all 50 planned cases defined in the evaluation document. Record a technical success only for a completed valid response, clarification or appropriate fallback; model/API errors and timeouts remain failures. | Technical success rate: technically successful requests / 50. | At least 95%, meaning at least 48/50. Correctness is assessed separately. |
| NFR4 | Maintainability | An approved source change shall be incorporable without retraining the language model or changing unrelated application components. | Run one controlled update to an approved source section; process the change and verify five queries covering the changed fact and unaffected facts. | Update time; number of verification cases passed. | At most 10 minutes, and 5/5 checks pass. |

The latency target balances a modest semester prototype with interactive use; the correctness target is an initial acceptance floor, not a claim of production readiness. Cost will be logged if a paid model is selected, and a budget requirement can then be added. A price limit is not invented before the access method and budget are known.

## Section 3 Responsible AI Requirements

| ID | Responsible AI risk | Requirement | Measurement method | Metric | Initial threshold |
|---|---|---|---|---|---|
| RAI1 | Unsupported or misleading answers | Substantive policy claims shall be traceable to approved evidence, with correct applicability and no fabricated references. | Review all policy claims and references produced for the answerable set against original source sections; evaluate citation existence separately from semantic support. | Citation presence rate across substantive document-grounded answers; supported-and-cited policy-claim rate across all produced policy claims. | 100% citation presence; at least 95% supported-and-cited claims. An empty or universally refusing system cannot pass NFR1. |
| RAI2 | Unsupported advice or misleading capability claims | The system shall acknowledge insufficient/conflicting evidence and respect its informational scope. | Run five unanswerable/conflict cases and five out-of-scope/action cases. Check for a correct boundary statement, no unsupported substantive answer and an appropriate official redirect where applicable. | Correct fallback/boundary rate; number of false claims that a renewal, reservation or payment was completed. | 10/10 appropriate responses; zero false completion claims. |
| RAI3 | Privacy leakage | The system shall avoid requesting credentials and prevent the defined test identifiers/secrets from reaching external model payloads or evaluation logs unnecessarily. | Use five synthetic privacy cases containing a labelled student ID, email, password or equivalent test secret. Inspect user responses, outbound payloads and retained logs; do not use real personal data. | Number of credential requests; number of unredacted test identifiers/secrets observed in external payloads or logs. | Zero in both metrics within this defined test set. This does not establish perfect protection for all possible input formats. |
| RAI4 | Outdated information and inappropriate transfer across contexts | Answers shall use an explicitly reviewed source version and preserve user/campus applicability. Conflicting or unapproved passages shall be withheld from definitive answers. | Audit source metadata and check all answerable and conflict-test responses for unsupported edition/user/campus transfer. Include a controlled conflicting-source case. | Approved passages with required provenance fields / all approved passages; definitive answers relying on unresolved conflicts or unsupported applicability. | 100% required provenance coverage; zero such definitive answers in the defined tests. |

Required provenance fields are source ID, title, original URL or publisher reference, edition when available, retrieval date, page/section locator, user/campus applicability and approval status. Record an unknown effective date as unknown rather than inventing one.

## Part A Reflection

RAI4, which addresses outdated information and inappropriate transfer across contexts, will be the most difficult requirement to measure and enforce technically. Retaining a source title, URL and retrieval date is straightforward, but those fields do not prove that a passage is still applicable to the current academic year, campus or user category. The UTM Library information is distributed across a current handbook, official webpages and older editions, and some differences may be only terminology changes while others may alter eligibility, procedures or charges. The system therefore needs an explicit source-precedence rule, applicability metadata and human review of material differences. Automated checks can detect missing provenance or a changed page, but they cannot reliably determine every semantic policy conflict without a reviewed reference answer or manual judgement.

## Links to the remaining Lab 2 work

- [Evaluation criteria](week2_evaluation_criteria.md)
- [V1 architecture](week2_architecture_v1.png)
- [Architecture decisions and traceability](week2_architecture_decisions.md)
- [Source register](week2_source_register.md)
