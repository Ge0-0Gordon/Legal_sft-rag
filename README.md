# Legal RAG

Lightweight legal-domain RAG system built with FastAPI, React, LangChain, ChromaDB, and local Ollama models. The main application code lives in `git-rag/legal_rag`.

This repository is intended to keep the runnable application, configuration, and data-preparation code in GitHub while excluding local datasets, vector databases, model checkpoints, training outputs, and personal documents.

## Features

- Local LLM and embedding workflow through Ollama.
- FastAPI backend with pluggable RAG pipeline components.
- React + Vite frontend for chat, knowledge-base management, and performance monitoring.
- Legal-text processing with structured chunking, contextual headers, HyDE, reranking, knowledge-graph enhancement, and self-reflection.
- Training and inference configuration examples for Qwen3 LoRA experiments under `LegalSFT/configs/`.

## Project Structure

```text
.
|-- git-rag/legal_rag/         # Main Legal RAG application
|   |-- backend/               # FastAPI backend
|   `-- frontend/              # React + Vite frontend
|-- LegalSFT/                  # SFT configs, notebooks, and local training assets
|   |-- configs/               # Training and inference YAML configs
|   `-- prepare_disc_law.ipynb # Dataset preparation notebook
|-- .gitignore                 # GitHub upload safety rules
`-- README.md                  # Repository overview
```

The following local-only paths are intentionally ignored by Git:

- `LegalSFT/DISC-Law-SFT/`, `LegalSFT/lf_data/`: downloaded and processed datasets.
- `LegalSFT/saves/`, `outputs/`, `checkpoints/`, `models/`: training outputs and model artifacts.
- `LegalSFT/LLaMA-Factory/`: local third-party checkout.
- `git-rag/legal_rag/backend/data/` and `git-rag/legal_rag/backend/chroma_db/`: runtime data and vector-store state.
- Personal documents such as PDFs, Word files, and PowerPoint files.

## Requirements

- Python 3.10+
- Node.js 18+
- Ollama
- Local Ollama models:

```powershell
ollama pull qwen3:8b
ollama pull bge-m3
```

## Backend Setup

```powershell
cd git-rag\legal_rag\backend
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
```

The backend reads configuration from environment variables or a local `.env` file in the backend directory. `.env` files are ignored by Git.

Useful defaults:

```text
OLLAMA_BASE_URL=http://localhost:11434
LLM_MODEL=qwen3:8b
EMBEDDING_MODEL=bge-m3
```

## Frontend Setup

Open a second terminal:

```powershell
cd git-rag\legal_rag\frontend
npm install
npm run dev
```

The Vite dev server runs at `http://localhost:5173` and proxies `/api` requests to `http://localhost:8000`.

## Data

Large legal datasets are not included in Git. Place runtime data under:

```text
git-rag/legal_rag/backend/data/
```

Then run the backend import or preparation scripts from `git-rag/legal_rag/backend/scripts/` as needed.

## Tests

Backend tests:

```powershell
cd git-rag\legal_rag\backend
pytest tests -v
```

Frontend build check:

```powershell
cd git-rag\legal_rag\frontend
npm run build
```

## Uploading to GitHub

This folder is not currently initialized as a Git repository. To upload it:

```powershell
git init
git add .
git status --short
git commit -m "Initial Legal RAG project"
git branch -M main
git remote add origin <your-github-repo-url>
git push -u origin main
```

Before committing, review `git status --short` carefully. The ignored folders contain large files and personal/local artifacts, so they should not appear in the staged file list.

## Notes

This project is for learning and research use. Legal datasets and third-party dependencies remain subject to their original licenses and terms.
