# UTM Library Services Assistant

> A planned document-grounded AI assistant that helps UTM undergraduate students understand library services and rules using official sources.

**Student:** Cheong Jiahui  
**Course:** AI Engineering  
**Status:** Week 1 project scoping; implementation has not started.

## Problem Statement

UTM students seeking library information may need to consult several official pages about membership, borrowing, renewal, and library rules. Questions that combine several conditions may require students to bring information together from different sections. This project proposes an AI assistant that understands natural-language questions, retrieves relevant official information, and provides concise answers with source references. Its usefulness will be tested against a keyword-search baseline.

## Evidence of the Problem

Evidence 1: UTM publishes membership information and library rules on separate official pages. This demonstrates that relevant information is distributed across multiple sources.

Sources: UTM Library, [Library Membership](https://library.utm.my/how-to-become-a-member/) and [Library Rules and Etiquette](https://library.utm.my/general-regulations/).

Evidence 2: The official FAQ includes questions about borrowing, renewal, and membership, and directs users to additional pages for some details. This provides a starting set of information needs for the project.

Source: UTM Library, [Library FAQs](https://library.utm.my/libfaq/).

## Initial Related Work / Existing Approaches

1. UTM Library FAQs — existing solution. Provides predefined questions, answers, and FAQ search.

Source: [UTM Library FAQs](https://library.utm.my/libfaq/).

Key limitation: Users must identify relevant entries and combine information themselves when their question covers several topics. This is an initial assessment of the reviewed page.

2. Lewis et al. (2020), Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks. Combines document retrieval with language generation.

Source: [Lewis et al. (2020)](https://arxiv.org/abs/2005.11401).

Key limitation for this project: Its general QA results do not establish performance on UTM library information; local data preparation and evaluation are still needed.

3. Asai et al. (2022), Evidentiality-guided Generation for Knowledge-Intensive NLP Tasks. Investigates whether retrieved passages contain evidence supporting an answer.

Source: [Asai et al. (2022)](https://arxiv.org/abs/2112.08688).

Key limitation for this project: Its training-based method adds implementation work beyond a basic semester prototype. The project will instead apply the principle through evidence checks and evaluation of unsupported answers.

## Engineering Gap / Limitation

The reviewed FAQ provides useful information, but students still need to locate relevant entries and combine conditions across sources. The practical engineering gap is to build and evaluate a small conversational interface that retrieves relevant evidence, provides traceable answers, asks for clarification when needed, and avoids unsupported answers. Whether it improves on keyword search remains an evaluation question.

## Project Objectives

O1: To design a document-grounded question-answering assistant for UTM undergraduate students using selected public library documents.

O2: To develop the assistant with retrieval-augmented generation, source references, clarification prompts, and fallback responses for unsupported questions.

O3: To evaluate answer correctness, evidence support, handling of ambiguous and unsupported questions, and response time using a predefined test set, with keyword search as a baseline.

## Target Users

UTM undergraduate students, especially new students seeking general information about library membership, borrowing, renewal, and library rules.

## Data Sources

Source 1: [UTM Library FAQs](https://library.utm.my/libfaq/)

Source 2: [UTM Library Membership](https://library.utm.my/how-to-become-a-member/)

Source 3: [UTM Library Rules and Etiquette](https://library.utm.my/general-regulations/)

## Responsible AI Risks and Controls

Risk 1 — Incorrect or outdated information: An incorrect renewal answer may cause a student to miss a required action.

Control: Provide source references, record source dates, and flag conflicting or missing evidence.

Risk 2 — Privacy: Users may enter passwords, student IDs, or personal borrowing information.

Control: Do not request credentials; minimise stored query data and remove personal information from evaluation records.

Risk 3 — Misleading capability claims: Users may assume the assistant has renewed or reserved a book.

Control: Clearly state that it provides instructions only and direct users to the official service.

## Project Scope

The first version will answer questions about membership, borrowing, renewal, and basic library rules using selected public documents. It will provide source references, ask for clarification when necessary, and state when the available sources do not support an answer.

It will not access student accounts, renew or reserve books, handle credentials, or provide real-time account or book availability information. This is a student prototype, not an official UTM Library service.

## Repository Structure

| Path | Purpose |
|---|---|
| `docs/` | Project scope, group observation sheet, and conceptual system map |
| `data/` | Data documentation and permitted project data |
| `src/` | Application source code |
| `notebooks/` | Colab notebooks and experiments |
| `evaluation/` | Test questions, evaluation criteria, and results |
| `prompts/` | Prompt templates and revisions |

## Week 1 Engineering Evidence

- [Group observation sheet — ChatGPT](docs/week1_observation_sheet.md)
- [Group conceptual system map — ChatGPT](docs/week1_system_map.png)
- [Individual project scope — UTM Library Services Assistant](docs/week1_project_scope.md)

The ChatGPT observation sheet and system map are the shared outputs of Parts A and B. They are retained as Week 1 engineering evidence and do not represent the architecture of this individual project.

## Weekly Progress

| Week | Focus | Deliverable | Status |
|---|---|---|---|
| W1 | Existing-system analysis and individual project scoping | Observation sheet, system map, project scope, README | Prepared; GitHub upload and course portal submission pending |

## Data and Repository Safety

Do not commit API keys, passwords, private student data, restricted university information, or documents that cannot be redistributed. Public source URLs are recorded above; public accessibility does not automatically grant redistribution rights. Keep credentials in local environment variables and exclude them from Git.
