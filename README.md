# RAG Pipeline with IBM Granite and LangChain 🚀🤖

A Retrieval-Augmented Generation (RAG) pipeline that grounds an LLM's answers in
a real source document — instead of letting it hallucinate, it retrieves
relevant passages first and answers only from those.

This version uses the **State of the Union address** as the knowledge base,
**IBM Granite** (via Replicate) as the LLM, and a local **Milvus Lite**
vector store for retrieval.

## Why this project exists

LLMs are powerful but can state things confidently that aren't true. RAG
fixes this by forcing the model to answer only from retrieved, verifiable
text — effectively giving it an open-book exam instead of a closed-book one.

## How it works

1. Download a source document (State of the Union address)
2. Split it into chunks sized to the embedding model's context window
3. Embed each chunk with an IBM Granite embedding model
4. Store the embeddings in a Milvus vector database
5. On a query, retrieve the most relevant chunks
6. Feed the retrieved chunks + question into the Granite LLM to generate a grounded answer

## Tech stack

- ⚡ IBM Granite (embeddings + `granite-4.0-h-small` for generation)
- 🛠️ LangChain
- 🧠 Milvus (Milvus Lite, local file-based)
- 🐍 Python 3

## Setup

**Option A — Google Colab (recommended)**

1. Click the "Open in Colab" badge at the top of the notebook.
2. Get a Replicate API token from https://replicate.com/account/api-tokens
3. In Colab, click the key icon 🔑 in the left sidebar → add a secret named
   `REPLICATE_API_TOKEN` → paste your token as the value → enable notebook access.
4. Run the cells top to bottom.

Using Colab Secrets instead of `getpass` means your token survives runtime
restarts — no more re-pasting it every time the session disconnects.

**Option B — Local Jupyter**

```bash
git clone <your-repo-url>
cd <your-repo>
pip install -r requirements.txt
cp .env.example .env
# open .env and paste your token in
jupyter notebook RAG_Pipeline_IBM_Granite_LangChain.ipynb
```

The notebook's token-setup cell will detect you're not in Colab and fall
back to a manual `getpass` prompt.

## Troubleshooting

**`401 Unauthorized` from Replicate**
- Your token has a trailing space/newline from copy-paste — `rag_pipeline.py`
  strips this automatically, but double check your `.env` file doesn't have
  quotes around the token.
- The token was revoked. Generate a fresh one.
- Your account doesn't have access/billing enabled for the specific Granite
  model — visit the model's page on Replicate while logged in to check.

**`REPLICATE_API_TOKEN` missing / not found**
- Make sure you copied `.env.example` to `.env` (not just edited the example
  file) and that `.env` sits in the same directory you're running the script
  from.

**Running in Google Colab instead of locally**
- Use Colab's built-in **Secrets** manager (key icon in the left sidebar) to
  store `REPLICATE_API_TOKEN` instead of `getpass` — it persists across
  runtime restarts, which plain `os.environ` does not.

## Certifications

- 🏅 IBM SkillsBuild — 1M1B AI + Sustainability Virtual Internship
- 🎓 IBM SkillsBuild — Unleashing the Power of AI Agents

## License

MIT
