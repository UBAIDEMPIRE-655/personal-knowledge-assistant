# Personal Knowledge Assistant

## What it does
A 3-agent CrewAI crew that answers questions from my own notes (`notes.txt`):
- **Researcher** searches my document using a RAG pipeline (chunking, sentence-transformers embeddings, cosine similarity).
- **Verifier** checks the claims found in the notes against the live web (DuckDuckGo search).
- **Writer** (no tools) combines both into one final answer.

The crew also uses memory (`memory=True`) so it can recall earlier questions across runs.

## How to run
1. Open the notebook in Google Colab.
2. Save your Groq API key as a Colab secret named `GROQ_API_KEY`.
3. Run the cells in order. When asked, upload `notes.txt` (or your own .txt / .pdf file).
4. Ask a question by changing the text inside `ask("...")`.

## Notes on setup (what I changed and why)
- Installed `litellm` because the newer CrewAI needs it to talk to Groq.
- Used `sentence-transformer` instead of `huggingface` as the memory embedder, because the `huggingface` option asked for a separate API key.
- Added a small retry in `ask()` because Groq's free tier has a tokens-per-minute limit.

## Sample questions and answers

**Q1: What is a merge conflict and how do I resolve it?**

A merge conflict happens when Git cannot automatically combine changes from two branches because the same lines in a file were edited differently in each branch. Git marks the file with conflict markers and pauses the merge so you can decide how to proceed.

To resolve it, open the conflicted file, edit the marked sections to keep the desired changes, and remove the conflict markers. After saving, mark the conflict as resolved (`git add <file>`) and continue the merge or commit the resolution.

**Q2: What is RAG and how does it work?**

Retrieval-Augmented Generation (RAG) lets a language model answer questions using external information. Documents are split into small chunks, each chunk is turned into an embedding that captures its meaning, and the embeddings are indexed. When a user asks a question, the system finds the chunks most similar to the query and gives them to the language model, which writes an answer based on that retrieved content.

**Q3: Why do we split documents into chunks in that process?**

In RAG, a long document is broken into smaller, self-contained chunks before encoding. Each chunk gets its own embedding, so the system can compare the query against many compact vectors instead of one huge one. This keeps retrieval fast, keeps the computation manageable, and lets the model focus on the most relevant sections.

**Q4: What is the latest stable version of Python?**

My notes said the latest stable version is Python 3.9, which is outdated. The Verifier searched the web and the final answer reported a newer 3.14 release instead of the version in my notes. This shows the Verifier catching an outdated claim. (Version number as output by the model.)

## Memory observation
I asked "What is RAG and how does it work?" and then, in a separate run, "Why do we split documents into chunks in that process?" The second question never mentions RAG, but the answer began with "In RAG...", which suggests it may have recalled the first run. However, I cannot be sure, because my notes also discuss chunking in the context of RAG, so the answer could have come from the retrieved notes instead of memory. The logs also showed "Query analysis failed, using defaults" warnings, so memory may have been running in a weaker mode.
