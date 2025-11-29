# Applied LLM Systems  
## Homework 1 – Code Plagiarism Detection with RAG Systems

## 🚀 Quick Start

### 1. Setup Environment

```bash
# Create virtual environment
python -m venv venv

# Activate virtual environment
# Windows:
venv\Scripts\activate

# Linux / macOS:
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt
```

---

### 2. Set API Keys

Required: set environment variables for API authentication.

**Windows (PowerShell)**
```powershell
$env:GITHUB_TOKEN="your_github_token_here"
$env:GEMINI_API_KEY="your_gemini_api_key_here"
```

**Linux / macOS (Bash)**
```bash
export GITHUB_TOKEN="your_github_token_here"
export GEMINI_API_KEY="your_gemini_api_key_here"
```

**How to obtain API keys**
- GitHub Token: https://github.com/settings/tokens (public_repo scope required)
- Gemini API Key: https://aistudio.google.com/app/apikey (free tier available)

⚠️ **Security Note:**  
API keys must never appear in notebooks, scripts, or committed files.

---

### 3. Run Notebooks in Order

#### Phase 1: Data Collection and Indexing (5–10 minutes)

```bash
jupyter notebook 01_indexing.ipynb
# Restart Kernel → Run All
```

This notebook:
- Downloads code from five GitHub repositories
- Extracts 399 Python functions
- Builds FAISS and BM25 indexes
- Creates 30 labeled test cases

---

#### Phase 2: Interactive Testing (2–3 minutes)

```bash
jupyter notebook 02_interactive.ipynb
# Restart Kernel → Run All
```

This notebook:
- Implements four plagiarism detection methods
- Loads pre-built indexes
- Provides an interactive testing interface

---

#### Phase 3: Evaluation and Analysis (10–15 minutes)

```bash
jupyter notebook 03_evaluation.ipynb
# Restart Kernel → Run All
```

This notebook:
- Evaluates all four systems on the test dataset
- Generates comparison visualizations
- Performs ablation studies
- Saves results to the results/ directory

---

## 🎯 Four Detection Methods Implemented

### 1. Pure Embedding Search
- Approach: CodeBERT embeddings with cosine similarity  
- Pros: Fast, scalable, no LLM usage  
- Cons: Sensitive to thresholds, weaker under heavy refactoring  
- Use case: Large-scale initial screening  

### 2. Direct LLM Analysis
- Approach: Entire retrieved context passed directly to Gemini  
- Pros: Strong semantic reasoning  
- Cons: Expensive, slow, context window limits  
- Use case: Small corpora, high semantic accuracy needs  

### 3. Standard RAG
- Approach: Embedding-based retrieval followed by LLM reasoning  
- Pros: Balanced cost and performance  
- Cons: Dependent on retrieval quality  
- Use case: Most practical plagiarism detection scenarios  

### 4. Hybrid RAG ⭐ (Best Performance)
- Approach: Dense embeddings + BM25 fusion before LLM inference  
- Pros: Most robust across transformations  
- Cons: Slightly higher computational complexity  
- Use case: Production-grade plagiarism detection  

---

## 📊 Results Summary

| Method | Accuracy | Precision | Recall | F1 Score |
|------|---------|-----------|--------|---------|
| Pure Embedding | 0.267 | 0.316 | 0.400 | 0.353 |
| Direct LLM | 0.400 | 0.444 | 0.800 | 0.571 |
| Standard RAG | 0.400 | 0.440 | 0.733 | 0.550 |
| **Hybrid RAG** | **0.500** | **0.500** | **0.933** | **0.651** |

### Key Findings
- Hybrid RAG achieves the highest overall F1 score
- Optimal retrieval depth: k = 5–7
- Optimal fusion weight: α = 0.5
- Most challenging plagiarism type: minor refactoring

---

## 🔬 Ablation Studies

### Effect of k (Number of Retrieved Documents)
- Tested k ∈ {1, 3, 5, 7, 10}
- Best performance observed for k = 5–7
- Lower k misses relevant context
- Higher k introduces noise and increases cost

### Effect of α (Fusion Weight)
- Tested α ∈ {0.0, 0.25, 0.5, 0.75, 1.0}
- Best performance at α = 0.5
- Confirms benefit of combining semantic and lexical retrieval

---

## 📋 Dataset Details

### Source Repositories
- TheAlgorithms/Python
- geekcomputers/Python
- trekhleb/learn-python
- karan/Projects-Solutions
- zhiwehu/Python-programming-exercises

### Test Dataset
- Total cases: 30
- Positive examples: 15 (real code with transformations)
- Negative examples: 15 (unrelated or fundamentally different code)

### Corpus Statistics
- 150 Python files downloaded
- 399 extracted functions (length-filtered)

---

## 🛠️ Technical Stack

### Core Libraries
- sentence-transformers (CodeBERT embeddings)
- faiss-cpu (vector similarity search)
- rank-bm25 (lexical retrieval)
- google-generativeai (Gemini API)
- scikit-learn (metrics)
- matplotlib & seaborn (visualization)

### Models
- Embedding Model: microsoft/codebert-base
- LLM: Google Gemini 2.5 Flash

---

## ⚠️ Known Issues and Limitations

- Gemini free-tier rate limits may slow evaluation
- Python-only implementation
- Dataset size intentionally small for controlled analysis

Pre-generated results are included to ensure consistent grading.

---

## 🔍 Troubleshooting

**GitHub token not found**  
Set the GITHUB_TOKEN environment variable before starting Jupyter.

**Gemini quota exceeded**  
Wait for rate-limit reset or use pre-generated results.

**FAISS index not found**  
Ensure 01_indexing.ipynb is run before other notebooks.

---

## 👤 Author

Name: Ana Guruli  
Course: Applied LLM Systems  
Assignment: Homework 1 – Code Plagiarism Detection with RAG Systems  
Date: 29/11/2025  

All notebooks run top-to-bottom using Restart and Run All.  
API keys are loaded from environment variables only.
