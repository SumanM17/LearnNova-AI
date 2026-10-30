# LearnNova AI

LearnNova AI is a local study assistant that turns PDF notes into searchable study material. Upload a PDF, ask questions about its contents, review a summary, or create exam practice materials from the document.

## Features

- **PDF library:** Upload and switch between PDFs. Documents are identified by their content and indexed in a persistent local ChromaDB database.
- **PDF reading:** Extracts selectable text from PDFs. Optional OCR can read scanned pages when its dependencies are installed and configured.
- **Ask Your Notes:** Search relevant passages and ask follow-up questions. Retrieved passages are available to inspect alongside the answer.
- **PDF Summary:** Build and download a Markdown summary with key points and definitions.
- **Exam Preparation Studio:** Generate mixed questions, MCQs, passage-based questions, point-wise revision notes, short-answer questions, long-answer questions, and flashcards. Choose a topic, difficulty, and other format options. MCQs can be answered interactively; flashcards are presented as Q&A cards.
- **Local AI:** Uses Ollama for chat and embeddings, and ChromaDB for local vector storage.

## Requirements

- Python and pip
- [Ollama](https://ollama.com/) installed and running
- The Ollama models used by the app:
  - `qwen2.5:3b` for chat and generated study materials
  - `nomic-embed-text` for document and question embeddings

Optional scanned-PDF OCR support requires PyMuPDF, pytesseract, Pillow, and the Tesseract OCR application.

## Setup

1. Install Ollama, then download the required models:

   ```powershell
   ollama pull qwen2.5:3b
   ollama pull nomic-embed-text
   ```

   Ensure the Ollama service is running before using the app.

2. From the project directory, create and activate a Python virtual environment:

   ```powershell
   python -m venv .venv
   .\.venv\Scripts\Activate.ps1
   ```

3. Install the project dependencies:

   ```powershell
   python -m pip install -r requirements.txt
   ```

4. Start LearnNova AI:

   ```powershell
   streamlit run app.py
   ```

   Streamlit will print a local URL to open in your browser.

### Optional OCR setup

Install the OCR dependencies:

```powershell
python -m pip install pymupdf pytesseract pillow
```

Install Tesseract OCR separately. The current app configuration points to:

```text
C:\Program Files\Tesseract-OCR\tesseract.exe
```

If Tesseract is installed elsewhere, update `pytesseract.pytesseract.tesseract_cmd` near the top of `app.py`. Text-based PDFs work without the optional OCR packages.

## Using the app

1. Upload a PDF with the file uploader.
2. Select the active document if you have uploaded more than one.
3. Use **Ask Your Notes** to ask document-specific questions and inspect the retrieved passages.
4. Use **PDF Summary** to generate, review, or download a summary.
5. Use **Exam Preparation Studio** to choose a study-material format, optionally enter a topic, set difficulty and item count, then generate material.

Generated answers and study materials are based on extracted content from the selected PDF. Review them against the original notes, especially if pages were scanned or text extraction reported errors.

## Project structure

| File or directory | Purpose |
| --- | --- |
| `app.py` | Main Streamlit application. |
| `requirements.txt` | Python dependencies for the main app. |
| `studymate_db/` | Persistent ChromaDB data created by the app; do not remove it if you want to keep indexed documents. |
| `LearnNova_AI_app.py`, `app_backup.py` | Other app versions/backups. Run `app.py` for the setup described here. |
| `read_pdf.py`, `chunk_pdf.py`, `embed_pdf.py`, `store_pdf.py`, `search_pdf.py`, `rag_chat.py`, `rag_chroma.py`, `search_chroma.py`, `embedding_test.py` | Standalone PDF, embedding, retrieval, and RAG helper or experiment scripts. |
| `app.zip` | Packaged project archive. |

## Implementation notes

- The main app uses Streamlit, Ollama, ChromaDB, and pypdf.
- ChromaDB stores data under `./studymate_db`; uploaded PDF text is split into overlapping chunks before embedding.
- The default chat model, embedding model, and embedding-versioned collection names are set near the top of `app.py`.
- The app expects the Ollama service and both configured models to be available locally.
