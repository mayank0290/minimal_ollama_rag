# Local Markdown RAG with Ollama

A lightweight, dependency-minimal Retrieval-Augmented Generation (RAG) script in Python. It indexes local Markdown (`.md`) files using vector embeddings and answers questions strictly based on your documents via a local [Ollama](https://ollama.com) instance.

## ✨ Features

* **100% Local & Private:** Runs entirely on your machine using Ollama—no external API keys or cloud services required.
* **Zero Heavy Frameworks:** Built purely with standard Python libraries, `requests`, and `numpy` (no LangChain or LlamaIndex overhead).
* **Cosine Similarity Search:** Performs fast, in-memory vector search using NumPy matrix operations.
* **Strict Context Grounding:** Instructs the LLM to answer *only* using retrieved document chunks to minimize hallucinations.

## 🛠️ Prerequisites

1. **Python 3.8+**
2. **[Ollama](https://ollama.com)** installed and running locally on `http://localhost:11434`.

## 🚀 Setup & Installation

### 1. Pull the Required Ollama Models
Before running the script, download the embedding and chat models specified in the code:

```bash
ollama pull nomic-embed-text
ollama pull qwen3:4b-instruct
```

### 2. Install Python Dependencies
Install `requests` and `numpy`:

```bash
pip install requests numpy
```

### 3. Add Your Documents
Create a `docs` folder in the root directory of the project and place your `.md` files inside it:

```text
project-root/
├── docs/
│   ├── notes.md
│   └── documentation.md
└── main.py
```

> **Note:** The script splits documents into chunks by double newlines (`\n\n`), so structure your Markdown paragraphs accordingly for optimal retrieval.

## 💻 Usage

1. Ensure the Ollama service is running in the background (`ollama serve`).
2. Run the Python script:

```bash
python main.py
```

3. Once you see `Indexed X chunks`, type your question at the `Ask:` prompt and press **Enter**.

### Example
```text
Indexed 42 chunks

Ask: How do I reset the configuration?

Based on the provided documentation, you can reset the configuration by deleting the `config.yaml` file and restarting the service.
```

## ⚙️ Configuration

You can customize the models, Ollama endpoint, or retrieval count directly at the top of the script:

| Variable | Default Value | Description |
| :--- | :--- | :--- |
| `OLLAMA` | `"http://localhost:11434"` | URL of your local Ollama server. |
| `EMBED_MODEL` | `"nomic-embed-text"` | Model used to generate text embeddings. |
| `CHAT_MODEL` | `"qwen3:4b-instruct"` | LLM used to generate final answers. |
| `k` (in `retrieve()`) | `3` | Number of top matching chunks passed as context. |

## 🗺️ Roadmap & Future Improvements

Ideas to make this script even more capable while keeping it lightweight:

- [ ] **Vector Caching:** Save embeddings to disk (`np.save`) and track file hashes so documents aren't re-embedded on every startup.
- [ ] **Smarter Chunking:** Split by Markdown headings (`#`, `##`) or character count with overlap to preserve context across paragraph boundaries.
- [ ] **Source Citations:** Track source filenames for each chunk and instruct the LLM to cite where each answer came from.
- [ ] **Streaming & Chat History:** Enable `"stream": True` for real-time token output and maintain multi-turn conversation context.
- [ ] **Similarity Score Threshold:** Filter out low-scoring chunks (e.g., `< 0.4`) to avoid passing irrelevant context on out-of-scope questions.
- [ ] **Expanded File Support:** Use `rglob()` for nested subfolders and add support for `.txt` and `.pdf` files.
