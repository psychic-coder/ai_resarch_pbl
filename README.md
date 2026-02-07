# 🏛️ AI-Driven Research Engine for Commercial Courts

An intelligent legal research assistant powered by **Claude Opus 4.5** that helps lawyers, judges, and legal professionals quickly search and analyze commercial court cases.

[![Explore the Demo](https://img.shields.io/badge/Explore%20the%20Demo%20-%E2%9C%94-green)](https://huggingface.co/spaces/hemanthkarthick03/Research-Agent-of-Commercial-Courts)

---

## 🚀 Quick Start

### Prerequisites

- **Python 3.9+** installed
- **OpenRouter API Key** (free at [openrouter.ai/keys](https://openrouter.ai/keys))

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/psychic-coder/ai_resarch_pbl.git
   cd ai_resarch_pbl
   ```

2. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Configure API Key**
   
   Edit the `.env` file and add your OpenRouter API key:
   ```
   OPENROUTER_API_KEY=your_openrouter_api_key_here
   ```

4. **Add PDF Documents** (optional)
   
   Place your legal PDF documents in the `data/` folder. A sample case is already included.

5. **Run the application**
   ```bash
   python3 app.py
   ```

6. **Open in browser**
   
   Navigate to: **http://localhost:7861**

---

## 📁 Project Structure

```
ai_resarch_pbl/
├── app.py                 # Main application
├── requirements.txt       # Python dependencies
├── .env                   # API key configuration
├── data/                  # PDF documents folder
│   └── Case-Test.pdf      # Sample legal case
├── vector_store/          # Generated embeddings (auto-created)
├── App.ipynb              # Original Jupyter notebook
└── README.md              # This file
```

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| **📄 PDF Processing** | Automatically loads and indexes all PDFs from `data/` folder |
| **🔍 Semantic Search** | FAISS-powered vector search for relevant document chunks |
| **🤖 Claude Opus 4.5** | State-of-the-art AI for accurate legal analysis |
| **📚 Source Citations** | Every answer includes page references |
| **🌐 Web Interface** | Beautiful Gradio chat interface |
| **📈 Scalable** | Add more PDFs anytime to expand knowledge base |

---

## 💬 Example Queries

Try asking:
- "What was the case of Manoj Kumar Pandey about?"
- "What does the Constitution say about right to appointment?"
- "Explain the concept of delay and laches in filing petitions"
- "What did the Supreme Court say about waiting list candidates?"

---

## 📈 Adding More Documents

1. Add your PDF files to the `data/` folder
2. Delete the `vector_store/` folder (to rebuild the index)
3. Restart the application

The system will automatically process and index all new documents.

---

## 🔧 Configuration

### Change the LLM Model

Edit `app.py` line 43 to use a different model:
```python
MODEL_NAME = "anthropic/claude-opus-4.5"  # Current model
# Other options:
# MODEL_NAME = "anthropic/claude-sonnet-4"
# MODEL_NAME = "meta-llama/llama-3.1-8b-instruct:free"  # Free option
```

### Adjust Chunk Size

For longer documents, you may want to adjust chunking in `app.py`:
```python
CHUNK_SIZE = 1000      # Characters per chunk
CHUNK_OVERLAP = 200    # Overlap between chunks
```

---

## 🛠️ Tech Stack

- **LangChain** - Document processing and retrieval chains
- **FAISS** - Vector similarity search
- **HuggingFace** - Sentence embeddings (`all-MiniLM-L6-v2`)
- **OpenRouter** - LLM API gateway (Claude Opus 4.5)
- **Gradio** - Web interface
- **PyPDF** - PDF document loading

---

## 👨‍💼 Target Users

- **Lawyers** - Research case law and precedents
- **Judges** - Quick reference to relevant cases
- **Legal Researchers** - Analyze commercial court documents
- **Law Students** - Study and understand legal concepts

---

## 📝 License

This project is open source and available under the MIT License.

---

## 🤝 Contributing

Contributions are welcome! Feel free to submit issues and pull requests.
