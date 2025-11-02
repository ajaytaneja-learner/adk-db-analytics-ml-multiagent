## 🧩 Agents Overview

- **Root Agent**: Acts as the entry point for user prompts. It interprets intent and routes tasks to the appropriate subagents or tools.
- **Database Agent**: Converts natural language to SQL using Gemini and queries BigQuery. I ahve used native Gemini 2.5 Flash for this.
- **Analytics Agent**: Converts natural language to Python code for data analysis and plotting. Receives data from the Database Agent.
- **BigQuery ML Agent**: Handles model training and inference using BigQuery ML. Always begins by querying a RAG corpus of the official BQML reference guide.



## 🧠 RAG Integration

The BQML Agent is grounded in documentation. It does not rely on LLM memory alone. Instead, it uses a pre-ingested RAG corpus containing the BigQuery ML reference guide. At runtime:

- It queries the corpus using `rag_response`.
- Retrieves the most relevant documentation snippets.
- Generates SQL based on the retrieved context.
- Presents the SQL to the user for review.
- Executes only after explicit user approval.

The RAG corpus is created once using `reference_guide_RAG/rag_setup.py` and referenced via the `BQML_RAG_CORPUS_NAME` in the `.env` file.

---

## ⚙️ .env Configuration

Create a `.env` file in the root directory with the following content:

```env
GOOGLE_GENAI_USE_VERTEXAI=1 #or 0 if you use API keys


# ML Dev backend config (optional)
GOOGLE_API_KEY= # if above is 0

# Vertex backend config
GOOGLE_CLOUD_PROJECT=
GOOGLE_CLOUD_LOCATION= # e.g. europe-west2 

# SQLGen method
NL2SQL_METHOD="BASELINE"  

# BigQuery Agent configuration
BQ_COMPUTE_PROJECT_ID=
BQ_DATA_PROJECT_ID=
BQ_DATASET_ID=

# RAG Corpus for BQML Agent
BQML_RAG_CORPUS_NAME='projects/xxxxxxxx/locations/europe-west4/ragCorpora/xxxxxxxx'

# Code Interpreter Extension (optional)
CODE_INTERPRETER_EXTENSION_NAME=''  # Leave empty , I didn't use code interpreter
# Models used in Agents
ROOT_AGENT_MODEL='gemini-2.5-flash' # or whatever you like
ANALYTICS_AGENT_MODEL='gemini-2.5-flash' # or whatever you like
BIGQUERY_AGENT_MODEL='gemini-2.5-flash' # or whatever you like
BASELINE_NL2SQL_MODEL='gemini-2.5-flash' # or whatever you like
CHASE_NL2SQL_MODEL='gemini-2.5-flash' # or whatever you like
BQML_AGENT_MODEL='gemini-2.5-flash' # or whatever you like



## 🧪 Python Environment Setup

To isolate dependencies and avoid conflicts, create a virtual environment inside the `data-science` directory and install the required libraries:

1. **Navigate to the data-science folder**
   ```bash
   cd python/agents/db-da-ds-multi-agent/data-science

2. python -m venv .venv

3. .venv\Scripts\activate

4. pip install -r requirements.txt

📚 ADK Documentation
For more details on agent setup, orchestration, and tooling, refer to the official ADK documentation



