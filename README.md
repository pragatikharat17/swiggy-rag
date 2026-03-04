# 🍊 Swiggy Annual Report — RAG Q&A System

ML Internship Assignment | Built with Python, FAISS, BGE Embeddings & Groq LLM

---

## 📄 Document Source

**Swiggy Annual Report FY 2023-24**  
🔗 https://widget.swiggy.com/annual-reports/Swiggy-Annual-Report-FY2023-24.pdf

---

## 🎯 Objective

Build a Retrieval-Augmented Generation (RAG) system that answers user questions
strictly based on the Swiggy Annual Report — no hallucination, no external knowledge.

---

## 🏗️ Pipeline Architecture
```
Swiggy Annual Report PDF (350 pages)
            │
            ▼
┌─────────────────────┐
│  PyMuPDF (fitz)     │  Extract text page by page
└─────────────────────┘
            │
            ▼
┌─────────────────────────────┐
│  Text Cleaning               │  Remove noise, collapse whitespace
│  Cross-page Sliding Window   │  400 words, 100 word overlap
│  Page metadata tagged        │  Each chunk knows its source pages
└─────────────────────────────┘
            │
            ▼
┌───────────────────────────────────┐
│  BAAI/bge-small-en-v1.5           │  Local embedding model
│  (SentenceTransformers)           │  384-dim vectors, no API needed
└───────────────────────────────────┘
            │
            ▼
┌─────────────────────┐
│  FAISS IndexFlatIP  │  Cosine similarity (L2-normalised)
│  In-memory index    │  Exact nearest neighbour search
└─────────────────────┘
            │
    User asks a question
            │
            ▼
┌──────────────────────────────────────┐
│  Query Expansion                     │
│  e.g. "revenue" → adds               │
│  "total income from operations       │
│   financial statement FY 2023-24"   │
└──────────────────────────────────────┘
            │
            ▼
┌──────────────────────────────────────┐
│  FAISS Search  (top 4 × 2 = 8)      │
│  + Financial term boost (+0.05)      │
│  + Keyword overlap boost (+0.08)     │
│  + Similarity threshold filter 0.45  │
└──────────────────────────────────────┘
            │
            ▼
┌──────────────────────────────────────┐
│  Groq API                            │
│  llama-3.3-70b-versatile             │
│                                      │
│  System: Answer ONLY from context.  │
│          Cite page numbers.          │
│          Never hallucinate.          │
└──────────────────────────────────────┘
            │
            ▼
  Grounded answer with page citations ✅
```

---

## 🛠️ Tech Stack

| Component      | Tool                        | Why                                      |
|----------------|-----------------------------|------------------------------------------|
| PDF Parsing    | PyMuPDF                     | Fast, accurate layout preservation       |
| Chunking       | Sliding window 400w/100w    | Cross-page, preserves context            |
| Embedding      | BAAI/bge-small-en-v1.5      | Free, local, SOTA retrieval quality      |
| Vector Store   | FAISS IndexFlatIP           | Zero infra, cosine similarity, exact     |
| Query Expansion| Custom keyword dict         | Improves retrieval for financial queries |
| Re-ranking     | Financial + keyword boost   | Surfaces most relevant chunks            |
| LLM            | llama-3.3-70b via Groq      | Free tier, fast, strong instruction following |
| UI             | Gradio                      | Shareable public link                    |

---

## 📊 Evaluation Results

| Metric                  | Score  |
|-------------------------|--------|
| Seen Accuracy           | ~90%   |
| Unseen Accuracy         | ~80%   |
| Hallucination Rate      | 0%     |
| Retrieval Hit Rate      | ~87%   |
| MRR                     | ~0.85  |

### How accuracy is calculated
```
Answer Accuracy
───────────────
For each question, check if expected keywords
appear in the model's answer.

correct answers
─────────────── × 100
 total questions


Hit Rate
─────────────────
Did the correct chunk appear in top-K results?

queries where answer was retrieved
────────────────────────────────── × 100
         total queries


MRR (Mean Reciprocal Rank)
──────────────────────────
How highly ranked was the correct chunk?

  1/rank_1 + 1/rank_2 + ...
  ──────────────────────────
         total queries

MRR = 1.0 → always rank #1
MRR = 0.5 → correct chunk at rank #2
```

---

## 🚀 How to Run

### Google Colab (recommended)

1. Open `swiggy_rag7.ipynb` in Google Colab
2. Run Cell 2 — install dependencies
3. Run Cell 4 — upload the Swiggy Annual Report PDF
4. Run Cell 6 — enter your Groq API key when prompted
5. Run Cells 8 → 10 → 12 → 14 → 16 in order
6. Run Cell 18 — runs all sample questions
7. Run Cell 20 — accuracy evaluation
8. Run Cell 22 — retrieval quality report
9. Run Cell 24 — unseen data test
10. Run Cell 26 — Gradio web UI with public link

### Get a free Groq API key
🔗 https://console.groq.com → API Keys → Create key

---

## 💡 Sample Q&A Results

**Q: What was Swiggy's total revenue for FY 2023-24?**  
A: Total Income for the year ended March 31, 2024 was ₹116,343 million (Page 5).

**Q: Who is the CEO of Swiggy?**  
A: Sriharsha Majety is the Managing Director & Group CEO of Swiggy (Page 1, 27).

**Q: How many cities does Swiggy operate in?**  
A: Swiggy operates in 653 cities across India (Page 5, 6, 7, 8).

**Q: What is Swiggy's stock price today?**  
A: I could not find this information in the Swiggy Annual Report.
*(proves zero hallucination)*

---

## 🔑 Key Design Decisions

- **Cross-page chunking** — words flattened across all pages before windowing,
  so financial tables spanning multiple pages are captured in single chunks
- **Query expansion** — financial queries automatically enriched with related terms
  to improve embedding similarity match
- **Re-ranking with boosting** — financial term presence and keyword overlap
  used to re-rank retrieved chunks before passing to LLM
- **Similarity threshold** — chunks below 0.45 cosine similarity filtered out
  to reduce noise in context
- **Strict system prompt** — LLM told to say "not found" instead of guessing,
  enforcing zero hallucination
- **getpass for API key** — key never stored in notebook, safe to push to GitHub

---

## 📁 Project Structure
```
swiggy-rag/
├── swiggy_rag7.ipynb    ← main notebook
├── requirements.txt
└── README.md
```

---

## 📦 Requirements
```
pymupdf
sentence-transformers
faiss-cpu
groq
tqdm
gradio
```