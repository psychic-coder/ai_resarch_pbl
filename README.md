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

## 1. What Is This Project?

This is a **fully offline, locally-running AI legal research assistant** built specifically for the **Commercial Courts of India**. It allows users to upload legal PDF documents (court judgments, acts, rules, case files) and ask natural language questions about them. The system retrieves the most relevant passages from those documents and generates a structured legal opinion — formatted exactly as a judge's clerk would write it.

**It requires no internet connection, no API keys, and no cloud services** after the initial model download. Everything runs on your own machine via Ollama.

---

## 2. Who Will Use This Project?

| User Type | How They Use It |
|---|---|
| **Lawyers / Advocates** | Quickly search case law and statutes without reading through hundreds of pages |
| **Judges & Judge's Clerks** | Research precedents and applicable law for pending matters |
| **Law Students** | Study acts, understand case structures, and explore legal reasoning |
| **Legal Researchers** | Analyse trends and precedents across a large collection of judgments |
| **Law Firms** | Build an internal knowledge base from their private case PDF library |
| **Government Legal Teams** | Search and interpret commercial legislation rapidly |

The system is designed for **non-technical legal professionals** — the UI is a simple chat interface. No coding knowledge is required to use it once set up.

---

## 3. Core Concept — What Is RAG?

The project is built on a technique called **Retrieval-Augmented Generation (RAG)**:

```
Instead of asking the AI to "know" law from training data (which can be wrong or outdated),
we give it YOUR documents and force it to only answer using what's in those documents.
```

This means:
- **No hallucination** of case names, section numbers, or judgments
- **Citable sources** — every answer includes the PDF filename and page number
- **Your documents = your knowledge base** — you control what the AI knows

---

## 4. Tech Stack — Every Tool Explained

### 4.1 Core AI / ML

| Tool | What It Is | Why We Use It |
|---|---|---|
| **Ollama** | Local LLM runtime | Runs AI models entirely on your machine — no API keys, no cloud |
| **phi3:mini** | Microsoft's small language model | Lightweight, fast, runs on a MacBook without a GPU |
| **phi3-legal** | Our custom model | `phi3:mini` with a legal persona baked in via a `Modelfile` |
| **mxbai-embed-large** | Embedding model by MixedBread AI | Purpose-built for retrieval tasks; converts text to vectors for search |

### 4.2 RAG Framework

| Tool | What It Is | Why We Use It |
|---|---|---|
| **LangChain** | AI orchestration framework | Wires together the loader → splitter → embedder → retriever → LLM pipeline |
| **LangChain LCEL** | LangChain Expression Language | Modern, composable way to build the chain (replaces old `RetrievalQA`) |
| **FAISS** | Vector similarity search library by Meta | Stores and searches document embeddings locally at high speed |
| **PyPDF** | PDF parsing library | Extracts raw text from legal PDF documents, page by page |

### 4.3 Application Layer

| Tool | What It Is | Why We Use It |
|---|---|---|
| **Gradio** | Python UI library for ML apps | Builds the chat interface in ~10 lines of code; includes a public share link |
| **python-dotenv** | Environment variable loader | Reads `.env` config file without hardcoding values |

### 4.4 Optional / Supporting Tools

| Tool | What It Is | Why We Use It |
|---|---|---|
| **Google API Python Client** | Google Drive SDK | Downloads PDFs from a shared team Drive folder automatically |
| **fpdf** | PDF generation library | Generates sample legal PDFs when real ones aren't available |
| **sentence-transformers** | HuggingFace embedding library | Available as an alternative embedding backend |

---

## 5. Internal Architecture — How Everything Works Together

### 5.1 High-Level Flow

```
┌─────────────────────────────────────────────────────────────────┐
│                        STARTUP (once)                           │
│                                                                 │
│  data/*.pdf  ──►  PyPDFLoader  ──►  Text Chunks (600 chars)    │
│                                           │                     │
│                              mxbai-embed-large (Ollama)         │
│                                           │                     │
│                              FAISS Vector Store (disk cache)    │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                      QUERY (every message)                      │
│                                                                 │
│  User Question                                                  │
│       │                                                         │
│       ▼                                                         │
│  MMR Retriever  ──►  Top 6 chunks from 20 candidates           │
│       │                                                         │
│       ▼                                                         │
│  PromptTemplate  ──►  "Context: {chunks}\nQuestion: {q}"       │
│       │                                                         │
│       ▼                                                         │
│  phi3-legal (Ollama)  ──►  Structured Legal Opinion            │
│       │                                                         │
│       ▼                                                         │
│  Source Citation  ──►  Filename + Page Number appended         │
│       │                                                         │
│       ▼                                                         │
│  Gradio Chat UI  ──►  Displayed to User                        │
└─────────────────────────────────────────────────────────────────┘
```

### 5.2 Step-by-Step Internal Pipeline

#### Step 1 — PDF Loading (`load_pdfs`)
- Scans the `data/` directory for `.pdf` files
- Uses `PyPDFLoader` to parse each PDF page-by-page
- Each page becomes a LangChain `Document` object with metadata: `{source: filename, page: N}`

#### Step 2 — Text Chunking (`build_vector_store`)
- `RecursiveCharacterTextSplitter` divides each document into overlapping chunks
- **Chunk size:** 600 characters (smaller = more precise retrieval)
- **Overlap:** 150 characters (prevents context from being cut off at boundaries)
- **Separators priority:** `\n\n` → `\n` → `.` → ` ` → `""` (tries to split at natural breaks first)

#### Step 3 — Embedding Generation
- Each chunk is passed to `mxbai-embed-large` running locally via Ollama
- The model converts each chunk into a **1024-dimensional vector** representing its semantic meaning
- These vectors are stored in a FAISS index on disk (`vector_store/`)
- A sentinel file `.ollama_embeddings` is created to mark the store as valid (prevents stale rebuilds)

#### Step 4 — Cached Vector Store (on subsequent runs)
- On restart, the app checks if `vector_store/index.faiss` and `.ollama_embeddings` both exist
- If yes → loads from disk (fast, ~1 second)
- If no → rebuilds from scratch (slow, ~1-2 minutes depending on document size)
- To force a rebuild: delete the `vector_store/` folder

#### Step 5 — MMR Retrieval
- When a user asks a question, it is embedded using the same `mxbai-embed-large` model
- **Maximal Marginal Relevance (MMR)** search is used instead of plain similarity search
- MMR fetches **20 candidate chunks**, then selects the **6 most diverse and relevant** ones
- This prevents the retrieved context from being repetitive (e.g., all from the same page)

#### Step 6 — Prompt Construction
- The 6 retrieved chunks are joined with `---` separators
- Injected into this prompt template:
  ```
  Context:
  {retrieved chunks}

  Question: {user question}

  Legal Opinion:
  ```

#### Step 7 — LLM Generation (`phi3-legal`)
- The prompt is sent to `phi3-legal` running via Ollama at `http://localhost:11434`
- **Temperature: 0.05** — extremely deterministic, no creative freedom
- **Context window: 4096 tokens** — fits 6 legal chunks + question comfortably
- **Max output: 1024 tokens** — enough for a full structured legal opinion

#### Step 8 — Structured Output
The `phi3-legal` model is instructed to **always** respond in this exact format:
```
**ISSUES:** [Core legal questions]
**APPLICABLE LAW:** [Cited act/section/case from context]
**ANALYSIS:** [Application of law to facts]
**CONCLUSION:** [Clear, reasoned decision]
```
If the context doesn't contain the answer, it responds with a fixed refusal message instead of guessing.

#### Step 9 — Source Citation
- After generating the answer, the retriever is called again to fetch source metadata
- PDF filename and page number are extracted from each retrieved chunk's metadata
- Appended to the final response as:
  ```
  ---
  Sources:
    Commercial_Courts_Act_2015.pdf (page 3)
    Arbitration_Act_1996.pdf (page 7)
  ```

---

## 6. The Custom Model — `phi3-legal`

This is not a standard off-the-shelf model. It is a **customised version of `phi3:mini`** built using an Ollama `Modelfile`.

### What the Modelfile Does

```
FROM phi3:mini                  ← Base model

PARAMETER temperature 0.05      ← Near-zero creativity (factual only)
PARAMETER num_predict 1024      ← Max response length
PARAMETER num_ctx 4096          ← Context window size

SYSTEM """
  You are a Legal Research Assistant for Commercial Courts of India.
  Rules:
  1. ONLY use facts from the provided CONTEXT
  2. NEVER hallucinate legal facts
  3. If context is insufficient → say "I cannot find..."
  Always format as: ISSUES / APPLICABLE LAW / ANALYSIS / CONCLUSION
"""
```

The system prompt is **baked into the model itself** — it cannot be overridden at runtime. This makes the legal persona permanent and consistent regardless of how the app prompts it.

---

## 7. File-by-File Reference

| File | Role | Key Details |
|---|---|---|
| `app.py` | **Main application** | RAG pipeline + Gradio UI. Entry point. |
| `Modelfile` | **LLM configuration** | Defines the `phi3-legal` custom model |
| `requirements.txt` | **Dependencies** | All Python packages needed |
| `.env` | **Config overrides** | `OLLAMA_BASE_URL`, `OLLAMA_MODEL`, `OLLAMA_EMBED_MODEL` |
| `generate_pdfs.py` | **Sample data generator** | Creates PDFs of Commercial Courts Act 2015 and Arbitration Act 1996 using fpdf |
| `generate_legal_book.py` | **Extended PDF generator** | Generates a longer legal reference book PDF |
| `scrape_legal_data.py` | **Web scraper** | Downloads official PDFs from government websites (with file size validation) |
| `drive_sync.py` | **Google Drive sync** | Downloads all PDFs from a shared Drive folder via OAuth |
| `credentials.json` | **Google Auth** | OAuth client ID/secret for Drive API (not committed to git) |
| `App.ipynb` | **Prototype notebook** | Early RAG experiments and step-by-step pipeline testing |
| `Model_Evaluation.ipynb` | **Evaluation notebook** | Benchmarks different embedding models and chunking strategies |
| `data/` | **Document store** | Drop PDFs here to add them to the knowledge base |
| `vector_store/` | **FAISS index** | Auto-generated. Delete to force re-indexing |

---

## 8. Data Management — 3 Ways to Add Documents

### Option A: Manual Drop
Drop any PDF into `data/`, delete `vector_store/`, restart `python app.py`.

### Option B: Generate Sample PDFs
```bash
python generate_pdfs.py
```
Creates two PDFs in `data/` with the full text of:
- The Commercial Courts Act, 2015
- The Arbitration and Conciliation Act, 1996 (as amended)

### Option C: Google Drive Sync
Best for teams sharing a document library:
```bash
python drive_sync.py
```
- Authenticates via OAuth (browser popup on first run, token cached after)
- Downloads all PDFs from the configured Drive folder to `data/`
- Skips files that already exist locally

---

## 9. Configuration Reference

All settings can be overridden in the `.env` file:

```env
OLLAMA_BASE_URL=http://localhost:11434    # Where Ollama is running
OLLAMA_MODEL=phi3-legal                   # Which LLM to use
OLLAMA_EMBED_MODEL=mxbai-embed-large      # Which embedding model to use
GOOGLE_DRIVE_FOLDER_ID=your_folder_id    # For Drive sync (optional)
```

Chunking settings are in `app.py`:
```python
CHUNK_SIZE    = 600   # Smaller = more precise hits, more chunks
CHUNK_OVERLAP = 150   # Higher = less context loss at boundaries
```

Retrieval settings in `app.py`:
```python
search_type = "mmr"        # Maximal Marginal Relevance
k           = 6            # Return top 6 chunks
fetch_k     = 20           # From 20 candidates
```

---

## 10. Privacy & Architecture Advantages

| Property | Detail |
|---|---|
| **100% Local** | No data ever leaves your machine |
| **No API Keys** | No OpenAI, Anthropic, or any cloud billing |
| **Private Documents** | Confidential case files stay on your device |
| **Offline-capable** | Works without internet after initial model pull |
| **No hallucination** | Model is constrained to only cite what's in the documents |
| **Auditable sources** | Every answer cites exact file and page number |

---

## 11. Evaluation & Research (`Model_Evaluation.ipynb`)

The project includes a separate Jupyter notebook that benchmarks the RAG pipeline across:
- Different embedding models (e.g., `mxbai-embed-large` vs HuggingFace `all-MiniLM-L6-v2`)
- Different chunking strategies (chunk size, overlap)
- Metrics: accuracy, precision, recall on a test question set
- Visualisations: confusion matrices, performance comparison charts

This is used for research/academic purposes to validate which configuration performs best on Indian commercial legal text.

---

## 12. System Requirements

| Component | Minimum | Recommended |
|---|---|---|
| **RAM** | 8 GB | 16 GB |
| **Storage** | 5 GB free | 10 GB free |
| **CPU** | Apple M1 / Intel i5 | Apple M2/M3 |
| **GPU** | Not required | Not required |
| **OS** | macOS 12+, Ubuntu 20+ | macOS 14+ |
| **Python** | 3.9 | 3.11–3.14 |
| **Ollama** | Latest | Latest |





## 📝 License

This project is open source and available under the MIT License.
