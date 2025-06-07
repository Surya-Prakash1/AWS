# AWS Bedrock RAG Demo

This project demonstrates how to use **AWS Bedrock** as the foundation for Retrieval-Augmented Generation (RAG) with large language models (LLMs). It showcases the integration of AWS Bedrock's managed LLMs and embedding models with a typical RAG pipeline, using PDF documents as the knowledge base and providing a question-answering interface via Streamlit.

---

## Table of Contents
- [Project Overview](#project-overview)
- [How It Works](#how-it-works)
- [Step-by-Step Guide](#step-by-step-guide)
- [AWS Bedrock Integration](#aws-bedrock-integration)
- [Requirements](#requirements)
- [Running the Project](#running-the-project)
- [Customizing for Your Data](#customizing-for-your-data)
- [References](#references)

---

## Project Overview

- **Goal:** Showcase how to build a RAG system using AWS Bedrock for both embeddings and LLM inference.
- **Tech Stack:**
  - [AWS Bedrock](https://aws.amazon.com/bedrock/) (LLMs & Embeddings)
  - [LangChain](https://python.langchain.com/)
  - [Streamlit](https://streamlit.io/)
  - [FAISS](https://github.com/facebookresearch/faiss) (Vector Store)
  - [PyPDF](https://pypdf.readthedocs.io/) (PDF Loader)

---

## How It Works

1. **Data Ingestion:**
   - PDF documents are loaded from a `data/` directory using LangChain's PyPDF loader.
   - Documents are split into manageable text chunks using a recursive character splitter.

2. **Embedding Generation:**
   - Each chunk is converted into a vector embedding using the **Amazon Titan Embeddings** model via AWS Bedrock.
   - Embeddings are stored locally using FAISS for fast similarity search.

3. **Query Processing:**
   - User inputs a question in the Streamlit UI.
   - The question is embedded using the same Titan model.
   - FAISS retrieves the most relevant text chunks based on vector similarity.

4. **LLM Response Generation:**
   - The retrieved context and the user query are sent to an LLM hosted on AWS Bedrock (e.g., Claude, Llama2, Jurassic).
   - The LLM generates a context-aware answer, which is displayed to the user.

---

## Step-by-Step Guide

### 1. AWS Setup
- **Create an AWS Account** if you don't have one.
- **Enable Bedrock Service** in your AWS region.
- **Configure AWS Credentials** (via `aws configure` or environment variables). Ensure your IAM user has permissions for Bedrock runtime.

### 2. Clone and Prepare the Project
```bash
# Clone the repo (or copy the folder)
cd "AWS bedrock"
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

### 4. Add Your Data
- Place your PDF files inside a folder named `data/` in the project root.

### 5. Run the Application
```bash
streamlit run app.py
```
- This will launch a local web app for interactive Q&A.

---

## AWS Bedrock Integration Details

### 1. **Embeddings with Amazon Titan**
- The code uses `BedrockEmbeddings` from LangChain, which wraps AWS Bedrock's Titan embedding model.
- It creates a Bedrock client with `boto3`:
  ```python
  bedrock = boto3.client(service_name="bedrock-runtime")
  bedrock_embeddings = BedrockEmbeddings(model_id="amazon.titan-embed-text-v2:0", client=bedrock)
  ```
- All document chunks and user queries are embedded using this model.

### 2. **LLM Inference via Bedrock**
- The code supports multiple LLMs available on Bedrock (e.g., Claude, Llama2, Jurassic).
- Example instantiation:
  ```python
  llm = Bedrock(model_id="meta.llama2-70b-chat-v1", client=bedrock)
  ```
- The LLM receives both the user query and the retrieved context, enabling RAG.

### 3. **Security**
- Credentials are never hardcoded. Use AWS CLI or environment variables for authentication.
- Make sure your IAM user/role has the correct Bedrock permissions.

---

## Requirements
See `requirements.txt` for all dependencies. Key packages:
- `boto3`, `awscli` (AWS SDK & CLI)
- `langchain`, `faiss-cpu`, `pypdf`
- `streamlit`
- `python-dotenv` (optional, for managing env variables)

---

## Customizing for Your Data
- To use your own PDFs, just place them in the `data/` folder.
- You can change the chunk size and overlap in `app.py` for different document types.
- You can switch to different Bedrock LLMs by changing the `model_id` in the LLM instantiation.

---

## References
- [AWS Bedrock Documentation](https://docs.aws.amazon.com/bedrock/latest/userguide/what-is-bedrock.html)
- [LangChain AWS Bedrock Docs](https://python.langchain.com/docs/integrations/providers/aws_bedrock)
- [FAISS](https://github.com/facebookresearch/faiss)
- [Streamlit](https://docs.streamlit.io/)

---

## About
This project was created to showcase the use of AWS Bedrock for end-to-end RAG workflows. It demonstrates real-world integration of AWS managed LLMs and embeddings for enterprise-ready retrieval-augmented applications.
