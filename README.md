# AWS Finance Operations Chatbot

An intelligent finance reporting assistant that uses generative AI and AWS services to make complex cost data easily queryable and interpretable. This project solves retrieval consistency issues in LLM-based systems by preprocessing financial data into structured, summary-rich formats.

---

## Overview

This chatbot is designed for internal employees working with AWS billing data and wishes to keep track, gain insights, and make informed decisions. 
It turns raw cost reports into meaningful insights, enabling users to ask natural-language questions and get accurate, explainable responses.

---

## End-To-End Workflow

- **Amazon S3** – Stores raw and transformed billing data
- **Amazon OpenSearch Serverless** – Vector search for LLM retrieval (RAG)
- **Amazon Bedrock** – Powers foundation models for response generation
- **Amazon Lex** – Front-end Integration for conversational user interface

---



## Proudest Achievement
## Problem Encountered

Traditional AWS billing reports are wide-format, dense, and difficult to navigate. Even when reshaped into long format, LLMs struggle with:
- Inconsistent data chunk retrieval
- Non-deterministic answers
- Poor performance on common financial metrics (QoQ, YTD, etc.)

---

## Objective Defined

To enable consistent and accurate LLM responses by transforming billing data into a semantically rich, vector-friendly format for generative search.

---

### Data Engineering

- Reorganized data into key financial views:
  - **Month-To-Date (MTD)**
  - **Quarter-To-Date (QTD)**
  - **Year-To-Date (YTD)**
  - **Department Cumulative**
  - **Service Cumulative**

- Automated the calculation of financial reporting metrics:
  - Total Cost, Cost %, QoQ/YoY Change
  - Financial summaries in natural-language format

### Retrieval Optimization

- Embedded text summaries into OpenSearch for RAG-style retrieval
- Tuned prompt design for summarization and metric interpretation
- Used Bedrock-hosted LLMs for accurate, warm-toned explanations

---

## Results

- The LLM now answers both simple and advanced finance questions with accuracy and consistency.
- Retrieval precision significantly improved due to granular summary text.
- Non-determinism is reduced through clear, structured data context.
