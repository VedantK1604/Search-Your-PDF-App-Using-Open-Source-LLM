# Search Your PDF — Open-Source LLM PDF Search App

A small demo application that lets you search the contents of a PDF using an open-source large language model (LLM) and local embedding/indexing. The repository contains a lightweight Python app to ingest a PDF, build an index, and answer queries over the document.

This README explains the project, lists prerequisites, and shows how to set up and run the app on Linux (bash).

## What this repository contains

- `app.py` — Main application entrypoint (query server / CLI).  
- `ingest.py` — PDF ingestion and indexing utilities.  
- `constants.py` — Configuration and constants used by the code.  
- `requirements.txt` — Python dependencies.  
- `docs/` — Example documents (a sample `Personal finance for dummies.pdf` is included).  
- `README.md` — This file.

## Goals and scope

This project is a simple, local-first demo to:
- Extract text from a PDF.
- Create vector embeddings of document chunks.
- Store an index locally and run semantic search.
- Ask natural-language questions about the PDF and get answers grounded in the document.

It is intended as a starting point for learning and experimentation with open-source LLMs and local retrieval. It is not production hardened (no authentication, limited error handling, and simple storage layout).

## Requirements

- Python 3.9+ (3.10+ recommended)
- pip
- Linux or Windows (instructions below assume Linux/bash)
- A CPU/GPU is optional — this app uses small open-source models or remote embedding providers depending on how you configure it.

## Setup (Linux / bash)

1. Create and activate a virtual environment (recommended):

```bash
python3 -m venv .venv
source .venv/bin/activate
```

2. Install dependencies:

```bash
pip install -r requirements.txt
```

3. (Optional) If you plan to add more PDFs, place them in the `docs/` directory or update `ingest.py` to point to your files.

4. Configure any constants in `constants.py` if needed (for example model choices, index locations, or API keys if you wire external services).

## Typical usage

1. Ingest a PDF and build the index:

```bash
python ingest.py
```

This will read the PDF(s) referenced in the ingestion script and build any local index files used by the app.

2. Run the app to query the document interactively (the exact CLI or server behaviour is implemented in `app.py`):

```bash
python app.py
```

Follow the on-screen prompts to enter a natural language question. The app will use the index and the configured model to return an answer with citations to the document.

## Configuration notes

- Open-source model: The codebase aims to be model-agnostic. Check `constants.py` for default settings. You can integrate local models (e.g., via Hugging Face or llama.cpp) or remote APIs. If you wire an external embedding or LLM provider, keep API keys out of source control and set them via environment variables.
- Index persistence: Index files are stored locally by default. Review `ingest.py` for exact paths and change them to suit your needs.

## Troubleshooting

- If text extraction fails, ensure the PDF is not scanned images or is protected. For scanned PDFs you may need OCR (e.g., Tesseract) before ingestion.
- If dependency installation fails, ensure your pip and Python versions are compatible and upgrade pip: `pip install --upgrade pip`.
- If the model or embeddings are missing, check `requirements.txt` and `constants.py` for which libraries or providers are expected.

## Extending this project

- Add more PDF files into `docs/` and update `ingest.py` so the index contains multiple documents.
- Swap in a different embedding model or LLM backend. Add configuration flags in `constants.py` and wire them into `ingest.py` and `app.py`.
- Add a small web UI or API layer that calls `app.py` logic and returns JSON responses.

## Security & privacy

This project stores indexes and may send text to whichever model/embedding provider you configure. Do not upload sensitive documents to third-party APIs without reviewing their privacy policy. Prefer local models if data must remain on-device.
