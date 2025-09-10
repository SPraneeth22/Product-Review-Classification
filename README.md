# Product-Review-Classification



This project performs **Product-Review-Classification** on product reviews to classify them as **positive** or **negative**. It follows a semi-supervised, rule-based, and lexicon-based approach to capture patterns in reviews, expand opinion words using external knowledge bases, and finally classify sentiment using both **linguistic rules** and **machine-learning-inspired scoring methods**.  

The project also integrates **OpenAI APIs with FAISS embeddings** for storing, retrieving, and classifying unseen reviews, allowing for an **adaptive and learning-based enhancement**.

---

## Project Goal
To classify product reviews as **positive** or **negative** using:
- Preprocessing pipelines
- N-gram generation
- Part-of-speech patterns
- Opinion target expansion using **WordNet**
- Sentiment scoring with **TextBlob**
- Semi-supervised categorization
- Optional **OpenAI-powered integration** for enhancement

---

## Workflow Breakdown

### 1. Data Preprocessing

- Expands contractions: e.g., `"won't"` → `"will not"`.
- Removes unwanted characters and punctuation.
- Converts reviews to lowercase for consistency.
- Removes **stopwords** using **NLTK’s stopwords list**.
- Applies **spelling correction** using **TextBlob**.
- Splits processed reviews into sentences and words.

### 2. Adding Biwords and Triwords

- Generates **n-grams**:  
  - Biwords (two-word sequences)  
  - Triwords (three-word sequences)  
- Captures contextual phrases like `"great phone"`, `"poor battery life"`.

### 3. POS Tagging

- Applies **Part-of-Speech (POS) tagging**.  
- Identifies grammatical structures useful for sentiment, e.g., adjective-noun combos.

### 4. Pattern Generation

- Defines multiple patterns (**pattern1–pattern8**) such as:
  - Adjective + Noun → `"amazing design"`
  - Verb + Object → `"love display"`
- Extracts **opinion-bearing structures**.

### 5. Initial Opinion Targets

- Defines a seed list of targets such as:
  - `"battery"`, `"software"`, `"display"`, `"camera"`.
- Represents product aspects users typically comment on.

### 6. Expanding Opinion Targets

- Uses **WordNet** to extract:
  - **Hypernyms** (general terms).
  - **Hyponyms** (specific terms).
- Expands `"display"` → `"screen"`, `"monitor"`, `"LCD"`.

### 7. Finding Opinion Words

- Matches **patterns** (step 4) with **opinion targets** (step 6).  
- Extracts opinion words → `"excellent"`, `"poor"`, `"broken"`.  

### 8. Expanding Opinion Words

- Uses **WordNet expansion** on adjectives/opinion words.  
- E.g., `"excellent"` → `"outstanding"`, `"brilliant"`.

### 9. Opinion Weight Calculation

- Uses **TextBlob polarity analysis**:
  - Positive → \( 0 \) to \( +1 \)  
  - Negative → \( 0 \) to \( -1 \)  
- Procedure:
  1. Find the **maximum absolute sentiment score** opinion word per review.
  2. Recalculate based on strongest terms.
  3. Compute **average score per review**.

### 10. Classification

- Reviews classified via threshold-based ranges:
  - Average score > threshold → **Positive**
  - Average score < threshold → **Negative**
- Generates `train_file.csv` with:
  - Original review
  - Opinion words
  - Sentiment scores
  - Final classification vs original rating

### 11. Evaluation

- Generates **Confusion Matrix**  
- Calculates:
  - **Accuracy**
  - **Precision**
  - **Recall**
  - **F1-score**

### 12. Testing on New Reviews

- Input: New review text.  
- Processed through pipeline:
  - Preprocessing → Opinion word detection → Scoring → Classification.  
- Output: **Predicted sentiment rating**.

### 13. OpenAI + FAISS Integration

- Uses **OpenAI’s embeddings + FAISS** for:
  - Generating text embeddings of reviews.
  - Storing feedback for similarity search.
  - Checking if given review already exists in DB.
- If review is **new**, pipeline:
  - Calls OpenAI API for **sentiment classification**.
  - Saves new embedding to FAISS index.

---

## Features
- **Semi-supervised sentiment classification**.
- **Editable lexicon expansion** via WordNet.
- **Context-aware n-grams** for better accuracy.
- **POS-based patterns** to capture nuanced sentiment.
- **Dual approach**:
  - Rule/lexicon based (traditional NLP).
  - LLM-enhanced classification via **OpenAI integration**.
- **Evaluation metrics** for model assessment.
- **Adaptive memory system** via FAISS for continuous learning.

---

## File Outputs
- **train_file.csv** → Stores metadata and computed labels.
- **Confusion matrix** output → Helps evaluate classification.
- **FAISS index storage** → Saves embeddings for efficient search.

---

## Tech Stack
- **Python 3.x**
- **Libraries:**
  - `NLTK` (stopwords, POS tagging, WordNet)
  - `TextBlob` (sentiment scoring, spelling correction)
  - `FAISS` (vector indexing)
  - `OpenAI` (LLM-powered classification and embeddings)
  - `NumPy`, `Pandas`, `Matplotlib` (data handling, evaluation, visualization)

---

## Example Output
**Input Review:**  
`"The battery life is amazing but the software is buggy."`

**Extracted Opinion Words:** `"amazing"`, `"buggy"`  
**Scores:** +0.8, -0.6  
**Final Average Score:** +0.1  
**Classification:** **Positive**

---

## Future Scope
- Add **neural-based sentiment classifier** (e.g., BERT, RoBERTa).  
- Improve **multi-class classification** (Positive, Negative, Neutral).  
- Incorporate **multi-lingual support**.  
- Use **topic modeling (LDA, BERTopic)** to auto-generate opinion targets.  
- Expand OpenAI use for **automatic lexicon enrichment**.  
- Build a **streamlit/web UI** for interactive review classification.

---

## How to Run
1. Clone repository
2. Run the cells one by one
