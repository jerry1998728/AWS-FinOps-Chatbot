# AWS Finance Operations Chatbot
## Overview
An intelligent finance reporting assistant that uses generative AI and AWS services to make complex cost data easily queryable and interpretable. This project solves retrieval consistency issues in LLM-based systems by preprocessing financial data into structured, summary-rich formats.


This chatbot is designed for internal employees working with AWS billing data and wishes to keep track, gain insights, and make informed decisions. 
It turns raw cost reports into meaningful insights, enabling users to ask natural-language questions and get accurate, explainable responses.



## End-To-End Pipeline

- **Amazon S3** – Stores raw and transformed billing data
- **Amazon OpenSearch Serverless** – Vector search for LLM retrieval (RAG)
- **Amazon Bedrock** – Powers foundation models for response generation
- **Amazon Lex** – Front-end Integration for conversational user interface

---


# Proudest Achievement
## Problems Encountered

Original AWS billing reports are wide-format, dense, and difficult to navigate. Even when reshaped into long format, LLMs struggle with:
- Inconsistent data chunk retrieval
- Non-deterministic answers
- Poor performance on common financial metrics (QoQ, YTD, etc.)

---

## Objective Defined

To enable consistent and accurate LLM responses by transforming cost data into a semantically rich, vector-friendly format for similarity search.

---

### Data Engineering

- Reorganized data into key financial views:
  - **Month-To-Date (MTD)**
  - **Quarter-To-Date (QTD)**
  - **Year-To-Date (YTD)**
  - **Department Cumulative**
  - **Service Cumulative**
<img width="572" alt="Screenshot 2025-05-01 at 14 22 14" src="https://github.com/user-attachments/assets/4aebdc57-740a-4fdf-89bf-096d690e65b0" />

- Automated the calculation of financial reporting metrics:
  - Total Cost, Cost %, QoQ/YoY Change
  - Financial summaries in natural-language format
<img width="1289" alt="Screenshot 2025-05-01 at 14 20 36" src="https://github.com/user-attachments/assets/03275577-b914-4cd4-bf92-40d9f21f61d8" />

### Default Chunking Strategy vs Optimized Chunking Strategy

After pre-processing and engineering the raw data in the best way, based on my corporate analytics and financial reporting experience, I re-evaluated the chunking strategy because the problems of inaccurate retrieval of data, non-deterministic answers, and hallucinations still exist.

With the default chunking strategy, LLM constantly and blindly mixes up or merges different records and summaries with the same date, due to blind slicing at ~300 tokens, regardless of the semantic meaning.
  - For example, when being asked "What is the total cost for the marketing department as of 10/31/2024?", the result would be something that has the same date mentioned but for some specific service not mentioned in the user query, or even mixes things up between department and time frames.

By exploring different chunking strategies, I discovered that the optimized way for my engineered and structured financial summaries is **Hierarchical Chunking**, with the advantages of:
  - Splits and respects the data structure
    - Parent node ()
    - Child node ()
  - Preserves Relationships and Reduces Mixing
    - semantically clean
    - contextually scoped (a single department in a single quarter)
    - linked to its parent (the overall quarterly report)
  - Efficient Token Management
    - set parent token sizes @ 1500 for broader context
    - set child token sizes @ 300 for precise granularity
    - set token overlap @ 60 to maintain continuity for edge queries


---

## Use Cases Validation & Evaluation
* Departmental Cost & Metrics Summary
  * <img width="654" alt="Screenshot 2025-05-07 at 15 00 55" src="https://github.com/user-attachments/assets/ee160665-3087-4823-9db9-2fe511dad27c" />

* Service Cumulative Cost & Metrics by Department
  * <img width="679" alt="Screenshot 2025-05-07 at 15 02 59" src="https://github.com/user-attachments/assets/e1ac7791-9e39-473c-b9a8-6e64da98b6c9" />

* Annual Cost & Metrics by Department
  * <img width="677" alt="Screenshot 2025-05-07 at 15 03 56" src="https://github.com/user-attachments/assets/e2261f32-8f36-41d3-b1eb-879288f9d017" />

* Quarterly Cost & Metrics by Department
  * <img width="690" alt="Screenshot 2025-05-07 at 15 04 42" src="https://github.com/user-attachments/assets/3d5e1533-024d-4642-a601-5fbfd6d84267" />

* Monthly Cost & Metrics by Department
  * <img width="675" alt="Screenshot 2025-05-07 at 15 13 17" src="https://github.com/user-attachments/assets/92f306ee-d914-4470-9601-dee640ac8fe1" />

* Sum Cost by Filtering for Specific Department and Service Type
  * <img width="678" alt="Screenshot 2025-05-07 at 15 16 11" src="https://github.com/user-attachments/assets/1ae03a55-101e-4a8a-969d-bbf8894ba8d3" />

* Sum Cost by Specific Department, Service Type, and Multiple Dates
  * <img width="679" alt="Screenshot 2025-05-07 at 15 20 05" src="https://github.com/user-attachments/assets/6573c83b-d95e-4f98-8672-7f990692171a" />












## As a Result
- The LLM now answers simple and advanced finance questions accurately and consistently.
- 100% Retrieval precision.
- 0% Non-determinism and hallucination.


