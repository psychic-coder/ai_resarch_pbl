# 🏛️ AI-Driven Research Engine for Commercial Courts

An intelligent legal research assistant that runs **100% locally** using [Ollama](https://ollama.com). No API keys or internet connection required after setup. Ask questions about commercial court cases and get structured legal opinions with cited sources.

---

## ⚙️ Prerequisites

Before you begin, make sure you have the following installed:

| Requirement | Version | Download |
|---|---|---|
| **Python** | 3.9+ | [python.org](https://www.python.org/downloads/) |
| **Ollama** | Latest | [ollama.com](https://ollama.com/) |

> **macOS users:** If `python3` isn't found, install it via Homebrew: `brew install python3`

---

## 🚀 Quick Start

### Step 1 — Clone the Repository

```bash
git clone https://github.com/psychic-coder/ai_resarch_pbl.git
cd ai_resarch_pbl
```

### Step 2 — Start Ollama

Open the **Ollama app** from your Applications folder, or run:

```bash
ollama serve
```

### Step 3 — Pull the Required Models

```bash
# Embedding model (for vector search)
ollama pull mxbai-embed-large

# Base LLM
ollama pull phi3:mini
```

### Step 4 — Create the Custom Legal Model

This builds a fine-tuned persona on top of `phi3:mini` using the included `Modelfile`:

```bash
ollama create phi3-legal -f Modelfile
```

You should see `success` at the end.

### Step 5 — Set Up Python Environment

```bash
# Create and activate a virtual environment
python3 -m venv venv
source venv/bin/activate       # On Windows: venv\Scripts\activate

# Install all dependencies
pip install -r requirements.txt
```

### Step 6 — Add Legal Documents

Place your PDF files into the `data/` folder. The app will automatically index them on first launch.

> **No PDFs?** Run the built-in generator to create sample legal texts:
> ```bash
> python generate_pdfs.py
> ```

### Step 7 — Run the Application

```bash
python app.py
```

Then open your browser at: **http://localhost:7862**

A public shareable link will also be printed in the terminal (powered by Gradio).

---

## 💬 Example Queries

Once running, try asking:

- *"What are the core factual disputes between the parties in this case?"*
- *"Identify the primary legal statutes and case precedents relied upon by the court."*
- *"What is the court's final ruling and the reasoning behind it?"*
- *"Explain the concept of delay and laches in filing petitions."*

---

## 📁 Project Structure

```
ai_resarch_pbl/
├── app.py                  # Main application (RAG pipeline + Gradio UI)
├── Modelfile               # Custom phi3-legal model definition
├── requirements.txt        # Python dependencies
├── .env                    # Optional environment variable overrides
├── data/                   # 📂 Place your PDF files here
├── vector_store/           # Auto-generated FAISS index (do not edit)
├── generate_pdfs.py        # Generates sample legal PDFs if data/ is empty
├── generate_legal_book.py  # Generates extended legal reference PDFs
├── scrape_legal_data.py    # Scrapes official legal PDFs from gov websites
├── drive_sync.py           # Syncs PDFs from a shared Google Drive folder
├── credentials.json        # Google OAuth credentials (for Drive sync only)
├── App.ipynb               # Jupyter notebook for experiments
└── Model_Evaluation.ipynb  # RAG evaluation and performance metrics
```

---

## 🛠️ Tech Stack

| Component | Technology |
|---|---|
| **LLM** | `phi3-legal` (custom Ollama model built on `phi3:mini`) |
| **Embeddings** | `mxbai-embed-large` via Ollama |
| **RAG Framework** | LangChain (LCEL) |
| **Vector Store** | FAISS (local) |
| **UI** | Gradio |
| **PDF Parsing** | PyPDF |

---

## 🔧 Configuration

You can override defaults by editing the `.env` file:

```env
OLLAMA_BASE_URL=http://localhost:11434   # Ollama server address
OLLAMA_MODEL=phi3-legal                  # LLM model name
OLLAMA_EMBED_MODEL=mxbai-embed-large     # Embedding model name
```

Or adjust chunking behaviour directly in `app.py`:

```python
CHUNK_SIZE    = 600   # Characters per chunk
CHUNK_OVERLAP = 150   # Overlap between chunks
```

---

## 📈 Adding More Documents

1. Add PDF files to the `data/` folder
2. Delete the `vector_store/` directory (forces a re-index):
   ```bash
   rm -rf vector_store/
   ```
3. Restart the app: `python app.py`

### Optional: Google Drive Sync

If your team stores PDFs in Google Drive:

1. Download `credentials.json` from [Google Cloud Console](https://console.cloud.google.com/)
2. Add `GOOGLE_DRIVE_FOLDER_ID=your_folder_id` to `.env`
3. Run: `python drive_sync.py`
4. Then start the app: `python app.py`

---

## 🧠 How It Works

```
PDF Files (data/)
      │
      ▼
  PyPDFLoader → RecursiveCharacterTextSplitter (600-char chunks)
      │
      ▼
  mxbai-embed-large (Ollama) → FAISS Vector Store
      │
      ▼
  User Query → MMR Retrieval (top-6 of 20 candidates)
      │
      ▼
  phi3-legal (Ollama) → Structured Legal Opinion
  [ISSUES / APPLICABLE LAW / ANALYSIS / CONCLUSION]
```

---

## 🐛 Troubleshooting

| Error | Fix |
|---|---|
| `Cannot reach Ollama` | Open the Ollama app or run `ollama serve` |
| `model "mxbai-embed-large" not found` | Run `ollama pull mxbai-embed-large` |
| `model "phi3-legal" not found` | Run `ollama create phi3-legal -f Modelfile` |
| `Address already in use` (port 7862) | Kill old process: `lsof -ti:7862 \| xargs kill` |
| `No PDFs in data/` | Add PDFs to `data/` or run `python generate_pdfs.py` |
| `ModuleNotFoundError` | Activate venv: `source venv/bin/activate`, then `pip install -r requirements.txt` |

---

## 📝 License

This project is open source and available under the MIT License.
