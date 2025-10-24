# Local RAG
Local RAG model finetuned with lightweight Google Colab T4 GPU

See the link to observe the finetuned results
https://colab.research.google.com/drive/1SbhFgW_pB_3X0K9MX8e1wdeqiAPBM5fP?usp=sharing


Task: To Build and enhance a robust real world Question Answering system.
Goal: Design a system that can *thoughtfully handle ambiguity* and *avoid common failures* through RAG and Finetuning.

Core features:
1. MULTIPLE SOURCES RAG: Retrieves information from two distinct and deliberately overlapping sources, prioritizes them, and cites its evidence
2. Finetuning the guardrail model: Determines if a question is answerable from the retrieved context in order to reduce model hallucination.


Source A (corpus): filtered_wikiqa.tsv
Source B (Source of Truth): Tables 1, 2, 3, and 4 [https://aclanthology.org/D15-1237.pdf](https://aclanthology.org/D15-1237.pdf)

Model used: `google/flan-t5-small`

### 1. Multi-Source RAG with Prioritization and Citation

**Goal:** Design a RAG system that can handle queries over two different sources and justify its answers.

*   **Data Ingestion:**
    *   Load the `wikiqa_sample.tsv` dataset
    *   Extract the content of all four tables (Tables 1-4) from the provided PDF.
*   **System Design:**
    *   Create a retrieval system that indexes all documents from both sources, while keeping the citation
    *   Implement a **prioritization logic** for retrieval.
    *   The final output for a question must include two things: the generated answer string and **a citation** indicating which document(s) and source(s) were used to generate it.


### 2. Finetuning for Answerability (Guardrail Model)

**Goal:** Finetune `flan-t5-small` to function as a post-retrieval check, preventing the system from answering questions when the retrieved context is irrelevant.

*   **Problem Formulation:**
    *   Build a model that takes a `question` and a `context` (the retrieved documents) and outputs one of two classifications: **`ANSWERABLE`** or **`UNANSWERABLE`**.
*   **Data Creation:**
    *   Create a small dataset (15-20 examples is sufficient) to finetune the model for this binary classification task. The dataset should contain *Positive examples (`ANSWERABLE`)* and *Negative examples (`UNANSWERABLE`)*
    *   Devise and **document** a strategy to create these.
*   **Finetuning & Demonstration:**
    *   Finetune `flan-t5-small` on the created dataset.
    *   Demonstrate the trained model on at least two examples: one where the provided context is relevant (and it outputs `ANSWERABLE`), and one where it is not (and it outputs `UNANSWERABLE`).

