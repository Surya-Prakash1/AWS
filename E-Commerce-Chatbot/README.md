# Ecommerce Chatbot

Welcome to the Ecommerce Chatbot project! This chatbot is designed to assist users with their online shopping experience by providing product recommendations, answering questions, and assisting with various inquiries related to the ecommerce store.

## Overview

The Ecommerce Chatbot is built using Python and Flask framework. It leverages natural language processing (NLP) techniques to understand user queries and generate appropriate responses. The chatbot integrates with the ecommerce store's product database to provide personalized recommendations and information about available products.

## Features

- Interactive chat interface for users to interact with the chatbot.
- Natural language processing for understanding user queries.
- Product recommendation engine based on user preferences and browsing history.
- Integration with the ecommerce store's product database.
- Ability to handle various user inquiries such as product availability, pricing, shipping information, etc.

## Installation

To set up the Ecommerce Chatbot locally, follow these steps:

1. Clone the repository to your local machine:
```
   git clone https://github.com/your-username/ecommerce-chatbot.git
```

2. Navigate to the project directory:
```
   cd ecommerce-chatbot
```

3. Install the required Python packages using pip:
```
pip install -r requirements.txt
```

4. Set up environment variables:
- Create a .env file in the project directory.
- Define the necessary environment variables such as database connection strings, API keys, etc.
  
5. Run the Flask application:
```
python app.py
```

---

## Codebase Documentation (`EcommerceChatBot`)

### 1. `data_converter.py`
**Purpose:**
Converts product review CSV data into a list of `Document` objects for use in NLP tasks and vector storage.

**Main Function:**
- `dataconveter()`: Reads a CSV of product reviews, processes each row into a dictionary with `product_name` and `review`, and converts these into LangChain `Document` objects with metadata. Returns a list of these documents.

**How it fits:**
Used as the data ingestion step, preparing data for vector storage and retrieval.

---

### 2. `ingest.py`
**Purpose:**
Handles ingestion of product review documents into an AstraDB vector store, embedding them for semantic search.

**Main Function:**
- `ingestdata(status)`: Sets up the AstraDB vector store with HuggingFace embeddings. If `status` is `None`, it calls `dataconverter()` from `data_converter.py` to get documents and adds them to the store. Otherwise, it returns the vector store instance. Also includes a script for testing ingestion and similarity search.

**How it fits:**
Responsible for storing and indexing product review data for downstream retrieval and chatbot generation.

---

### 3. `retrieval_generation.py`
**Purpose:**
Defines the retrieval-augmented generation (RAG) pipeline for the chatbot, combining vector search with LLM-based response generation.

**Main Function:**
- `generation(vstore)`: Sets up a retrieval chain using the vector store, a prompt template, and an OpenAI chat model. Returns a chain that takes a user question and retrieves relevant context before generating a response.

**How it fits:**
Implements the core logic for generating chatbot responses based on retrieved product reviews and user queries.

---

### 4. `__init__.py`
**Purpose:**
Marks the directory as a Python package. (No logic inside.)

**How it fits:**
Allows the `EcommerceChatBot` directory to be imported as a module in your project.

---

## AWS Deployment

Follow these steps to deploy your Ecommerce Chatbot on AWS EC2:

### 1. Push your entire code to GitHub

Make sure all your code is committed and pushed to a GitHub repository.

### 2. Login to your AWS account

Go to [AWS Console](https://aws.amazon.com/console/).

### 3. Launch your EC2 Instance

- Choose Ubuntu as your OS for compatibility with the following commands.

### 4. Configure your EC2 Instance

#### Update the package index:
```sh
sudo apt-get update
sudo apt update -y
```
- `sudo apt-get update` uses the traditional package management tool.
- `sudo apt update -y` uses the newer, user-friendly interface for APT.

#### Install required tools:
```sh
sudo apt install git curl unzip tar make sudo vim wget -y
```

### 5. Clone your GitHub repository
```sh
git clone <your-repo-url>
```

### 6. Create a .env file
```sh
touch .env
```

Open the file in VI editor:
```sh
vi .env
```
- Press `i` to insert, add your environment variables, then press `Esc`, type `:wq` and press `Enter` to save and exit.
- Check the contents:
```sh
cat .env
```

### 7. Install Python and pip
```sh
sudo apt install python3-pip
```

### 8. Install requirements
```sh
pip3 install -r requirements.txt
pip3 install -r requirements.txt --break-system-packages
```
- The `--break-system-packages` flag allows pip to override the externally-managed-environment error and install Python packages system-wide.
- To install any additional package system-wide:
```sh
pip install package_name --break-system-packages
```

### 9. Run your application
```sh
python3 app.py
```

### 10. Configure your inbound rule
- Go to your EC2 instance's Security Group settings.
- Click on Security Group, then edit the Inbound Rules.
- Add a rule for:
  - **Port:** 5000
  - **Source:** 0.0.0.0/0 (for anywhere traffic)
  - **Protocol:** TCP

Save the rule. Now your application should be accessible on port 5000.

---
