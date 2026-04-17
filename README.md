# 🔬 Paperlytics CLI

> **AI-powered research paper analyzer — right in your terminal.**  
> Multi-model RAG, embedded chatbot, offline support. Built by [Sanjeevni Dhir](https://linkedin.com/in/sanjeevnidhir).

[![PyPI version](https://img.shields.io/pypi/v/paperlytics?color=00ff9d&style=flat-square)](https://pypi.org/project/paperlytics)
[![Python](https://img.shields.io/badge/python-3.9%2B-blue?style=flat-square)](https://python.org)
[![License](https://img.shields.io/badge/license-MIT-f5a623?style=flat-square)](LICENSE)
[![GitHub](https://img.shields.io/badge/GitHub-sanju234--san-181717?style=flat-square&logo=github)](https://github.com/sanju234-san)

---

## What It Does

Paperlytics is a local CLI tool that:

- Analyzes research papers (PDF or scanned images) using a custom RAG pipeline
- Lets you query any paper through an **interactive chatbot session** in your terminal
- Supports **multiple AI models** — LLaMA 3, Claude, DeepSeek, GPT-4o — switchable mid-session
- Works **offline** with a local Ollama fallback when no API key is set
- Saves all analyses locally so you can revisit them anytime

---

## Requirements

| Dependency | Version | Notes |
|---|---|---|
| Python | 3.9+ | Required |
| pip | 22.0+ | For installation |
| Tesseract OCR | 5.x | For scanned/image PDFs |
| Ollama (optional) | Latest | For offline/local mode only |
| OpenRouter API key (optional) | — | For cloud models |

---

## Install

```bash
pip install paperlytics
```

That's it. The `paperlytics` command is now available globally.

To verify:

```bash
paperlytics --version
```

---

## Quick Start

### 1. Set your OpenRouter API key (optional but recommended)

```bash
paperlytics config set-key
# Paste your key when prompted — stored securely in ~/.paperlytics/config.json
```

Get a free key at [openrouter.ai](https://openrouter.ai).

### 2. Analyze a paper

```bash
paperlytics analyze research_paper.pdf
```

With a specific question:

```bash
paperlytics analyze research_paper.pdf --question "What is the main contribution?"
```

With a specific model:

```bash
paperlytics analyze research_paper.pdf --model claude
```

### 3. Start the chatbot

```bash
paperlytics chat
```

This opens an interactive session. Load a paper and start asking questions:

```
[no paper loaded] > /load research_paper.pdf
[llama3 | research_paper.pdf] > What datasets were used?
[llama3 | research_paper.pdf] > /model claude
[claude | research_paper.pdf] > Summarize the methodology section
[claude | research_paper.pdf] > /exit
```

---

## All Commands

```
paperlytics analyze <file>       Analyze a PDF or image file
paperlytics chat                 Start an interactive chatbot session
paperlytics models               List available AI models
paperlytics history              View past analyses
paperlytics config set-key       Save your OpenRouter API key
paperlytics config show          Show current config
paperlytics --help               Show help
```

---

## Chatbot Commands (inside `chat` mode)

| Command | What it does |
|---|---|
| `/load <filepath>` | Load a paper into context |
| `/model llama3` | Switch to LLaMA 3 |
| `/model claude` | Switch to Claude (Anthropic) |
| `/model deepseek` | Switch to DeepSeek |
| `/model gpt4o` | Switch to GPT-4o |
| `/clear` | Reset the conversation context |
| `/history` | Show questions asked in this session |
| `/exit` | Quit the chatbot |

---

## Supported Models

| Model | Provider | Best For |
|---|---|---|
| `llama3` | Meta via OpenRouter | Default — fast, general use |
| `claude` | Anthropic via OpenRouter | Nuanced reasoning, long papers |
| `deepseek` | DeepSeek via OpenRouter | Technical/research-heavy papers |
| `gpt4o` | OpenAI via OpenRouter | Complex multi-step questions |
| `local` | Ollama (on-device) | Offline, no API key needed |

List models at any time:

```bash
paperlytics models
```

---

## Offline / Local Mode

If no OpenRouter API key is configured and Ollama is installed:

```bash
ollama pull llama3
paperlytics analyze paper.pdf --model local
```

Paperlytics automatically detects Ollama and routes requests locally. No internet required.

---

## Analyze Output Example

```
$ paperlytics analyze transformer_paper.pdf

✦ Paperlytics v2.0
  Model     : LLaMA 3 (via OpenRouter)
  File      : transformer_paper.pdf
  Mode      : Cloud RAG

  Extracting text...          ✓ 14,302 characters
  Chunking document...        ✓ 47 chunks
  Building vector index...    ✓ Indexed
  Generating summary...       ✓

──────────────────────────────────────────────────
  SUMMARY
──────────────────────────────────────────────────
  This paper introduces the Transformer architecture,
  proposing attention mechanisms as a replacement for
  recurrent layers in sequence transduction tasks...

  KEYWORDS
  attention mechanism · transformer · encoder-decoder ·
  multi-head attention · positional encoding

  PLAGIARISM ESTIMATE   18% similarity detected

  Saved to: ~/.paperlytics/analyses/2025-06-01_transformer.json
──────────────────────────────────────────────────
```

---

## File Support

| Format | Support |
|---|---|
| `.pdf` | Full text extraction via pdfplumber |
| `.png` `.jpg` `.jpeg` | OCR via Tesseract |
| `.bmp` `.tiff` | OCR via Tesseract |

---

## Config File

Stored at `~/.paperlytics/config.json`:

```json
{
  "openrouter_api_key": "sk-or-...",
  "default_model": "llama3",
  "save_analyses": true,
  "analyses_dir": "~/.paperlytics/analyses/"
}
```

Edit directly or use `paperlytics config set-key`.

---

## Analyses Storage

Every analysis is saved locally as a JSON file:

```
~/.paperlytics/
├── config.json
└── analyses/
    ├── 2025-06-01_transformer_paper.json
    ├── 2025-06-03_bert.json
    └── ...
```

View past analyses:

```bash
paperlytics history
paperlytics history --open transformer_paper   # re-open a specific one
```

---

## Install Tesseract (for scanned PDFs)

**macOS:**
```bash
brew install tesseract
```

**Ubuntu / Debian:**
```bash
sudo apt install tesseract-ocr
```

**Windows:**
Download the installer from [UB-Mannheim/tesseract](https://github.com/UB-Mannheim/tesseract/wiki).

---

## Uninstall

```bash
pip uninstall paperlytics
rm -rf ~/.paperlytics    # optional — removes saved config and analyses
```

---

## Troubleshooting

**`command not found: paperlytics`**
Your Python scripts directory may not be in `PATH`. Try:
```bash
python -m paperlytics --version
# or add ~/.local/bin to PATH in your shell profile
```

**`Tesseract not found`**
Install Tesseract for your OS (see above). Then retry.

**`OpenRouter API error`**
Check your key is valid and has credits. Run `paperlytics config set-key` to update it.

**`No answer found`**
The question may not match any content in the paper. Try rephrasing or using `/clear` to reset context.

---

## Built With

- [Typer](https://typer.tiangolo.com/) — CLI framework
- [FastAPI](https://fastapi.tiangolo.com/) — backend API layer
- [ChromaDB](https://www.trychroma.com/) — vector store
- [sentence-transformers](https://www.sbert.net/) — local embeddings
- [pdfplumber](https://github.com/jsvine/pdfplumber) — PDF extraction
- [Tesseract OCR](https://github.com/tesseract-ocr/tesseract) — image text extraction
- [OpenRouter](https://openrouter.ai) — multi-model LLM gateway

---

## Author

**Sanjeevni Dhir** — GenAI Engineer  
B.Tech CSE (Data Science & AI), SRM University Delhi-NCR  
[sanjeevnidhir05@gmail.com](mailto:sanjeevnidhir05@gmail.com) · [LinkedIn](https://linkedin.com/in/sanjeevnidhir) · [GitHub](https://github.com/sanju234-san)

---

## License

MIT — free to use, modify, and distribute.
