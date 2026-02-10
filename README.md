# Samsung Mobile AI Support Assistant 📱🤖

An intelligent, RAG-based customer support agent designed to assist users in finding the perfect Samsung smartphone. Built with **n8n**, **OpenAI**, **Pinecone**, and **Docker**.

## 📖 Project Overview

This project implements a Retrieval-Augmented Generation (RAG) pipeline that acts as a specialized sales and support assistant for Samsung mobile devices. It allows users to ask natural language questions about phone specifications, compare models, and get pricing information (converted to JOD) based on a structured dataset.

## 📸 Workflow Diagrams

### 1. Data Ingestion Pipeline
![Data Ingestion Workflow](assets/Data-preparation.png)
*This workflow handles fetching data from Google Drive, processing it, and embedding it into Pinecone.*

### 2. AI Agent Chat Interface
![AI Agent Workflow](assets/AI-agent.png)
*This workflow manages the chat interface, memory, and retrieval (RAG) process.*

**Key Features:**
* **RAG Architecture:** Retains factual accuracy by grounding responses in a vectorized dataset of phone specs.
* **Currency Localization:** Data pre-processed to display prices in Jordanian Dinar (JOD).
* **Context Awareness:** Remembers the last 10 interactions for natural follow-up questions.
* **Smart Comparison:** Automatically compares technical specs (RAM, Battery, Processor) when asked for recommendations.

## 🛠️ Tech Stack

* **Workflow Automation:** [n8n](https://n8n.io/)
* **LLM:** OpenAI `gpt-4o-mini`
* **Embeddings:** OpenAI `text-embedding-3-large`
* **Vector Database:** [Pinecone](https://www.pinecone.io/)
* **Deployment:** Docker Desktop (Local)

## 📂 Project Structure

This repository contains two main n8n workflow JSON files:

1.  **`1_Data_Ingestion.json`**:
    * Fetches the dataset from **Google Drive**.
    * Parses binary data using the **Default Data Loader**.
    * Splits text into chunks (Size: 500, Overlap: 80).
    * Embeds and upserts data into **Pinecone**.

2.  **`2_AI_Agent_Chat.json`**:
    * Hosts the **AI Agent** with a specific system prompt.
    * Connects to the **Pinecone** vector store tool.
    * Manages conversation memory (Window Buffer = 10).

## 🚀 Prerequisites

Before running this project, ensure you have the following installed and set up:

1.  **Docker Desktop:**
    * Required to run n8n locally.
    * [Download Docker Desktop](https://www.docker.com/products/docker-desktop/)

2.  **API Keys:**
    * **OpenAI API Key:** For the LLM and Embedding models.
    * **Pinecone API Key:** For the vector database.
    * **Google Cloud Console (Optional):** If you plan to re-run the Google Drive ingestion, you will need OAuth2 credentials for the Google Drive node in n8n.

3.  **Pinecone Index:**
    * Create a serverless index named `samsung-mobiles` (or update the workflow with your index name).
    * Dimension: **3072** (Matches `text-embedding-3-large`).

## 💻 Installation & Execution

### Step 1: Run n8n via Docker

Open your terminal or command prompt and run the following command to start n8n:

```bash
docker run -it --rm --name n8n -p 5678:5678 -v ~/.n8n:/home/node/.n8n n8nio/n8n
```
Once the container is running, open your browser and go to: `http://localhost:5678`

### Step 2: Import Workflows
1. Download the `.json` files from this repository.
2. n your n8n dashboard, click **Add Workflow** > **Import from File**.
3. Import both `1_Data_Ingestion.json` and `2_AI_Agent_Chat.json`.

### Step 3: Configure Credentials
In n8n, go to the **Credentials** section and add:
* **OpenAI API**: Enter your API Key.
* **Pinecone API**: Enter your API Key.
* **Google Drive OAuth2**: (Only needed for the Ingestion workflow).

### Step 4: Ingest Data (Run Once)
1. Open the **Data Ingestion** workflow.
2. Ensure your dataset file (Samsung Mobiles) is in the connected Google Drive.
3. Click **Execute Workflow**.
4. *Verification*: Check your Pinecone dashboard to confirm that vectors have been upserted.

### Step 5: Start the Assistant
1. Open the **AI Agent Chat** workflow.
2. Click **Chat** (or Execute) to open the testing interface.
3. Ask a question: "*I need a phone with a 6000mAh battery for under 200 JOD.*"

## 📝 Dataset

The dataset used is the **Samsung Mobiles Dataset** sourced from Kaggle.
* **Modifications**: The `Price` column was converted from INR to JOD for this specific implementation.

## 🤝 Contributing
Feel free to fork this repository and submit pull requests. Any improvements to the prompt engineering or data pipeline are welcome!
