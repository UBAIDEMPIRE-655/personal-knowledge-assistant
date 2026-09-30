# Personal Knowledge Assistant

## What it does
A 3-agent CrewAI crew that answers questions from my own notes (`notes.txt`):
- **Researcher** searches my uploaded document using a RAG pipeline (chunking + sentence-transformers embeddings + cosine similarity).
- **Verifier** checks the found claims against the live web using DuckDuckGo search.
- **Writer** (no tools) combines both into one final answer.
The crew uses `memory=True` with a HuggingFace embedder so it can recall earlier questions across runs.

## How to run
1. Open `Personal_Knowledge_Assistant.ipynb` in Google Colab.
2. Save your Groq key as a Colab secret named `GROQ_API_KEY`.
3. Run the cells in order; when asked, upload `notes.txt` (or your own .txt / .pdf).
4. Change the question in `crew.kickoff(inputs={"question": "..."})` and run.

## Sample questions and answers
**Q1:** What is a merge conflict and how do I resolve it?
**A1:** <paste your real output here>

**Q2:** What is RAG and how does it work?
**A2:** <paste your real output here>

**Q3:** What is the latest stable version of Python?
**A3:** <paste your real output here — note whether the Verifier corrected the outdated note>

## Memory observation
<Write 2-3 sentences about what you actually saw when you asked two related questions in separate runs: did the second answer show any sign of recalling the first?>
