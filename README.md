## 🚀 How to Run

### Prerequisites
- Google account (for Colab)
- Free Groq API key → https://console.groq.com

---

### Step 1 — Clone the repo
```bash
git clone https://github.com/pragatikharat17/swiggy-rag.git
cd swiggy-rag
```

### Step 2 — Open notebook in Google Colab

Option A — Direct upload:
```
1. Go to colab.research.google.com
2. File → Upload notebook
3. Upload swiggy_rag7.ipynb
```

Option B — From GitHub:
```
1. Go to colab.research.google.com
2. File → Open notebook → GitHub tab
3. Paste: https://github.com/pragatikharat17/swiggy-rag
4. Select swiggy_rag7.ipynb
```

### Step 3 — Get a free Groq API key
```
1. Go to https://console.groq.com
2. Sign up for free
3. API Keys → Create new key
4. Copy the key (starts with gsk_...)
```

### Step 4 — Run the notebook cells in order

| Cell | What it does |
|------|-------------|
| Cell 2  | Install dependencies |
| Cell 4  | Upload Swiggy Annual Report PDF |
| Cell 6  | Enter Groq API key |
| Cell 8  | Load & chunk PDF |
| Cell 10 | Build embeddings & FAISS index |
| Cell 12 | Setup retriever |
| Cell 14 | Setup LLM generator |
| Cell 16 | Ask function |
| Cell 18 | Run sample questions |
| Cell 20 | Accuracy evaluation |
| Cell 22 | Retrieval quality report |
| Cell 24 | Unseen data test |
| Cell 26 | Launch Gradio UI |

### Step 5 — Upload the PDF when prompted
```
Download from:
https://widget.swiggy.com/annual-reports/Swiggy-Annual-Report-FY2023-24.pdf

Then upload when Cell 4 asks for it.
```

### Step 6 — Launch Gradio UI
```
Cell 26 will print a public URL like:
https://xxxxxxxx.gradio.live

Open it in any browser — no setup needed.
```

---

### ⚡ Quick Start (TL;DR)
```
1. Open swiggy_rag7.ipynb in Colab
2. Get free Groq key from console.groq.com
3. Run all cells top to bottom
4. Upload PDF when asked
5. Enter Groq key when asked
6. Open Gradio link printed by last cell
```

## 📊 Evaluation Results

| Metric | Score |
|--------|-------|
| Seen Accuracy (in-sample) | 90.9% (10/11) |
| Unseen Accuracy (out-of-sample) | 90.0% (9/10) |
| Generalisation Gap | 0.9% 🟢 excellent |
| Hallucination Rate | 0% |
| Grade | 🟢 Excellent |

## 🎥 Demo & Sample Output

📁 **Google Drive — Demo Folder**  
🔗 [Click here to view sample output & demo video](https://drive.google.com/drive/folders/1WcEFlzHnb-FN2SFS0lgyGyZc7twP8E2o?usp=sharing)