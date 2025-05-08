---
# AWS Finance Operations Chatbot
## Overview
An intelligent finance reporting assistant that uses generative AI and AWS services to make complex cost data easily queryable and interpretable.
This chatbot is designed for internal employees working with AWS billing data and wishes to keep track, gain insights, and make informed decisions. 
It turns raw cost reports into meaningful insights, enabling users to ask natural-language questions and get consistently accurate, explainable, and ground-truth-based responses.



## End-To-End Pipeline
- **Amazon S3** – Stores raw and transformed billing data
- **Amazon OpenSearch Serverless** – Vector search for LLM retrieval (RAG)
- **Amazon Bedrock** – Powers foundation models for response generation and evaluation with a knowledge base connected to S3
- **Amazon Lex** – Front-end Integration for conversational user interface
- **Foundation Model**
  - Selected Claude Haiku 3.7 (Sonnet Variant)
    - High efficiency in cost and latency
    - Solid context window length (~200K tokens)
    - Human-like output tone and fluency
    - Safety by default
- **Manual Foundation Model Evaluation**

  
---


# Proudest Achievement
## Problems Encountered
Original AWS billing reports are wide-format, dense, and difficult to navigate.
- Raw Data
  - <img width="888" alt="Screenshot 2025-05-07 at 15 33 06" src="https://github.com/user-attachments/assets/7a20defa-de41-4076-a45f-e752e86fc60d" />

Even after reshaping into long format, LLMs still struggle with:
- Inconsistent data chunk retrieval
- Non-deterministic response
- Hallucination and mixture with similar time frames data (QoQ, YTD, etc.)


## Objective Defined
To enable consistent and accurate LLM responses by transforming cost data into a semantically rich, vector-friendly format for similarity search.


### Data Engineering
- Reorganized into key financial reporting perspectives:
  - **Month-To-Date (MTD)**
  - **Quarter-To-Date (QTD)**
  - **Year-To-Date (YTD)**
  - **Department Cumulative**
  - **Service Cumulative**
  - <img width="888" alt="Screenshot 2025-05-01 at 14 22 14" src="https://github.com/user-attachments/assets/4aebdc57-740a-4fdf-89bf-096d690e65b0" />

- Automated the calculation of financial reporting metrics:
  - Total Cost, Cost %, QoQ/YoY Change
  - Financial summaries in natural-language format
  - <img width="888" alt="Screenshot 2025-05-01 at 14 20 36" src="https://github.com/user-attachments/assets/03275577-b914-4cd4-bf92-40d9f21f61d8" />


### Default Chunking Strategy vs Optimized Chunking Strategy
After pre-processing and engineering the raw data using my corporate analytics and financial reporting experience, I re-evaluated the chunking strategy because the problems of inaccurate retrieval of data, non-deterministic answers, and hallucinations continued during evaluations.

With the default chunking strategy, LLM constantly and blindly mixes up or merges different records and summaries with the same date, due to blind slicing at ~300 tokens, regardless of the semantic meaning.
  - For example, when asking "What is the total cost for the marketing department as of 10/31/2024?", the result would be something that has the same date mentioned but for some specific service not mentioned in the user query, or even mixes things up between department and time frames.

By exploring different chunking strategies, I discovered that the optimized way for my engineered and structured financial summaries is **Hierarchical Chunking**, with the advantages of:
  - Splits and respects the data structure.
    - For example, the Month-To-Date data would be chunked by:
      - Parent node (The Reporting Month)
      - Child node (Department)
    - Or the Service Cumulative data would be chunked by:
      - Parent node (AWS Service)
      - Child node (Department)
  - Preserves Relationships and Reduces Mixing
    - semantically clean
    - contextually scoped (a single department in a single quarter)
    - linkage between parent and child nodes
  - Efficient Token Management
    - set parent token sizes @ 1500 for broader context
    - set child token sizes @ 300 for precise granularity
    - set token overlap @ 60 to maintain continuity for edge queries


## As a Result
- The LLM now answers simple and complex finance reporting and operational questions with consistent accuracy.
- 100% Retrieval Precision.
- 0% Non-Determinism and Hallucination.


---


## Use Cases Validation & Evaluation with Ground Truth Comparison
* Departmental Cost & Metrics Summary
  * Ground Truth:
    <img width="888" alt="Screenshot 2025-05-07 at 16 17 22" src="https://github.com/user-attachments/assets/2ce3b774-4623-432a-be3f-1dc626d938cd" />
  * Chatbot Response:
    <img width="888" alt="Screenshot 2025-05-07 at 15 00 55" src="https://github.com/user-attachments/assets/ee160665-3087-4823-9db9-2fe511dad27c" />

* Service Cumulative Cost & Metrics by Department
  * Ground Truth:
    <img width="888" alt="Screenshot 2025-05-07 at 16 16 51" src="https://github.com/user-attachments/assets/ac564e63-4c85-4ed3-adf4-cd19617a0ea8" />
  * Chatbot Response:
    <img width="888" alt="Screenshot 2025-05-07 at 15 16 11" src="https://github.com/user-attachments/assets/1ae03a55-101e-4a8a-969d-bbf8894ba8d3" />

* Annual Cost & Metrics by Department
  * Ground Truth:
    <img width="888" alt="Screenshot 2025-05-07 at 16 19 35" src="https://github.com/user-attachments/assets/4d2a17ce-a426-4ac9-a8d8-942869bd6f74" />
  * Chatbot Response:
    <img width="888" alt="Screenshot 2025-05-07 at 15 03 56" src="https://github.com/user-attachments/assets/e2261f32-8f36-41d3-b1eb-879288f9d017" />

* Quarterly Cost & Metrics by Department
  * Ground Truth:
    <img width="888" alt="Screenshot 2025-05-07 at 16 24 45" src="https://github.com/user-attachments/assets/84653b72-8765-4de7-9e35-cc526e6a3710" />
  * Chatbot Response:
    <img width="888" alt="Screenshot 2025-05-07 at 15 04 42" src="https://github.com/user-attachments/assets/3d5e1533-024d-4642-a601-5fbfd6d84267" />

* Monthly Cost & Metrics by Department
  * Ground Truth:
    <img width="888" alt="Screenshot 2025-05-07 at 16 27 47" src="https://github.com/user-attachments/assets/9d9d1dbf-72be-4ee9-af9e-b1e78015666d" />
  * Chatbot Response:
    <img width="888" alt="Screenshot 2025-05-07 at 15 13 17" src="https://github.com/user-attachments/assets/92f306ee-d914-4470-9601-dee640ac8fe1" />

* Sum Cost by Filtering for Specific Department, Service Type, and Multiple Dates
  * Ground Truth:
    <img width="888" alt="Screenshot 2025-05-07 at 16 31 55" src="https://github.com/user-attachments/assets/39bdc8bc-5539-4191-b30c-4884773adbd8" />
  * Chatbot Response:
    <img width="888" alt="Screenshot 2025-05-07 at 15 20 05" src="https://github.com/user-attachments/assets/6573c83b-d95e-4f98-8672-7f990692171a" />















