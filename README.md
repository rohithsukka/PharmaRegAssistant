# Build a Complete PharmaRegAssistant with LLMs, LangChain, Pinecone and Flask

## Overview

A small Flask app that uses LangChain and a vector store (Pinecone) to build a context-aware medical/pharma assistant. The app converts documents to embeddings, stores them in Pinecone, and serves a simple chat UI that retrieves relevant documents and uses an LLM to answer queries.

---

## Requirements

* Python 3.10 or newer
* An OpenAI API key (or another embedding/LLM provider)
* A Pinecone API key and index
* `uv` (optional) or `pip` for package management

---

## Quickstart

### 1. Clone the repository

```bash
git clone https://github.com/rohithsukka/PharmaRegAssistant.git
cd PharmaRegAssistant
```

### 2. Create and activate a virtual environment

You can use `uv` (recommended if installed) or plain `python -m venv`.

Using `uv`:

```bash
uv venv --python 3.10
# On Linux/macOS
source .venv/bin/activate
# On Windows PowerShell
.\.venv\Scripts\Activate.ps1
# On Windows cmd
.\.venv\Scripts\activate.bat
```

Using `python -m venv`:

```bash
python -m venv .venv
# then activate as shown above
```

### 3. Install dependencies

```bash
uv pip install -r requirements.txt
# or
pip install -r requirements.txt
```

### 4. Create a `.env` file in the project root

Add your Pinecone and OpenAI credentials (no spaces around `=`):

```ini
PINECONE_API_KEY="your_pinecone_api_key"
OPENAI_API_KEY="your_openai_api_key"
```

Adjust any other environment variables your code expects.

### 5. Build and store embeddings

Run the script that converts documents under `data/` into embeddings and uploads them to Pinecone:

```bash
python store_index.py
```

If your documents change, re-run this command to refresh the index.

### 6. Run the app

```bash
python app.py
```

Open the UI at `http://127.0.0.1:5000` (or the address printed by `app.py`).

---

## Project structure

* `app.py`: Flask application serving the chat UI and API endpoints.
* `store_index.py`: Script to create/store embeddings into Pinecone (or another vector store).
* `requirements.txt`: Python dependencies.
* `setup.py`: package metadata (optional).
* `src/`:

  * `src/helper.py`: utility functions used by the app.
  * `src/prompt.py`: prompt templates and prompt-related helpers.
* `templates/chat.html`: chat UI template.
* `static/style.css`: basic styling for the UI.
* `data/`: sample documents and source data for indexing.

---

## How it works (short)

* `store_index.py` reads documents from `data/`, converts them to embeddings using your configured embedder, and uploads them to a Pinecone index.
* `app.py` accepts user messages from the UI, runs a vector similarity search against the index, and uses a chat/completion LLM to generate context-aware answers.

---

## Development tips

* To iterate on prompts, edit `src/prompt.py` and reload the app.
* If you change documents in `data/`, re-run `python store_index.py` to refresh the index.
* Add logging or debug statements in `app.py` and `store_index.py` for troubleshooting API calls and responses.
* If a loader fails to read certain file types, install optional extras (for example `pymupdf`, `unstructured`, or other dependencies used by your document loaders).

---

## Troubleshooting

* If you see `ModuleNotFoundError`, ensure you installed dependencies in the same Python environment Jupyter or your runtime uses. Check `sys.executable` in Python to confirm.
* If Pinecone operations fail, verify `PINECONE_API_KEY`, environment, and index name are correct and that the index exists.
* For LangChain loader import errors with your pinned versions (0.3.x), use `langchain_community` for community loaders and `langchain.text_splitter` for the splitter.

---

## Tech stack

* Python
* LangChain
* Flask
* OpenAI (or other LLM providers)
* Pinecone
* Provide exact `uv` or `pip` install commands that include optional extras
* Add a sample `.env.example` file to the repo
