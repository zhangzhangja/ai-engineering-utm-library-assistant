# UTM Library Services Assistant

> A planned AI assistant that answers UTM students' library service questions using official documents and source references.

## Problem Statement

UTM students may need to consult several official pages to find information about library membership, borrowing, renewal, and rules. This project will use retrieval-augmented generation to answer natural-language questions and combine relevant information with source references. Its usefulness will be evaluated against a keyword-search baseline.

## Evidence of the Problem

- **Evidence 1:** Membership information and library rules are published on separate official pages.  
  **Sources:** UTM Library, [Library Membership](https://library.utm.my/how-to-become-a-member/) and [Library Rules and Etiquette](https://library.utm.my/general-regulations/).

- **Evidence 2:** The official FAQ covers borrowing, renewal, and membership, and refers users to additional pages for some details.  
  **Source:** UTM Library, [Library FAQs](https://library.utm.my/libfaq/).

## Initial Related Work / Existing Approaches

- **[UTM Library FAQs](https://library.utm.my/libfaq/):** Provides predefined answers and FAQ search; users must combine relevant entries themselves for questions covering several topics.
- **[Lewis et al. (2020), Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks](https://arxiv.org/abs/2005.11401):** Combines retrieval and generation; performance on UTM library information still requires local evaluation.
- **[Asai et al. (2022), Evidentiality-guided Generation for Knowledge-Intensive NLP Tasks](https://arxiv.org/abs/2112.08688):** Studies whether retrieved passages support an answer; its training-based method adds complexity beyond the initial prototype.

## Project Objectives

- **O1:** To design a document-grounded question-answering assistant for UTM undergraduate students using selected public library documents.
- **O2:** To develop the assistant with retrieval-augmented generation, source references, clarification prompts, and fallback responses for unsupported questions.
- **O3:** To evaluate answer correctness, evidence support, handling of ambiguous and unsupported questions, and response time using a predefined test set, with keyword search as a baseline.

## Target Users

UTM undergraduate students, especially new students seeking general information about library membership, borrowing, renewal, and library rules.

## Data Sources

- [UTM Library FAQs](https://library.utm.my/libfaq/)
- [UTM Library Membership](https://library.utm.my/how-to-become-a-member/)
- [UTM Library Rules and Etiquette](https://library.utm.my/general-regulations/)

## Weekly Progress

| Week | Focus | Deliverable | Status |
|---|---|---|---|
| W1 | System analysis and project scoping | README, [observation sheet](docs/week1_observation_sheet.md), [system map](docs/week1_system_map.png), and [project scope](docs/week1_project_scope.md) | Files uploaded to GitHub; course portal submission pending |
