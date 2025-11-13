# Genomic Text Curation & Topic Grouping  
This project performs simple genomic entity extraction and topic clustering on a collection of scientific abstracts related to Alzheimer’s disease. The goal is to curate key biological information—such as variants, genes, and associated phenotypes—and to group the publications into meaningful thematic clusters using NLP techniques.

---

## 1. Dataset
The dataset consists of ~50 abstracts obtained from the ADVP/NIAGADS publication list.  
You will find the input file here:

- **texts.csv** — cleaned table with:  
  - `ID`  
  - `Title`  
  - `Abstract`  

These texts focus on Alzheimer’s disease genetics, GWAS results, biomarkers, and related biomedical findings.

---

## 2. Entity & Relation Extraction
We extract the following fields from each abstract:

| Field | Why It Matters |
|-------|----------------|
| **variant** | Identifies specific SNPs (e.g., `rs429358`) linked to AD risk. |
| **gene** | Highlights biologically relevant genes mentioned in the text. |
| **phenotype** | Shows what disease/trait the publication is studying. |
| **relation** | Captures interaction patterns such as “associated with” or “increases risk of.” |
| **evidence_span** | The exact sentence supporting the relation, improving interpretability. |

Extraction is done using rule-based NLP:
- Regex for variants (`rs\d+`)
- Uppercase detection for gene symbols
- Capitalized multi-word diseases
- Simple relation keyword matching
- Sentence-level evidence extraction using spaCy

The curated final output is stored in:

- **curated_output.json**

---

## 3. Topic Modeling (TF-IDF + Clustering)
To understand common themes across the publications, we applied two unsupervised topic methods:

### **(1) TF-IDF + KMeans**
- We vectorized abstracts using TF-IDF.  
- We tested various `k` values using an elbow plot.  
- KMeans produced broad topic clusters covering:
  - GWAS & genetics  
  - APOE-related findings  
  - Biomarker/tau pathology  
  - Polygenic risk & population studies  

**Plot:**  
- `kmeans_inertia.png`

### **(2) TF-IDF + NMF (Non-negative Matrix Factorization)**
NMF tends to give cleaner, more interpretable topics.  
Top keywords for each topic were extracted and interpreted.

**Plot:**  
- `nmf_topics.png`

These results help summarize the research landscape in a simple way.

---

## 4. Limitations
- Extraction method is mostly rule-based, so it sometimes misses scientific phrases that don’t follow the patterns I expect.
- Gene detection can be messy because many all-caps words (like MRI or DNA) look like gene symbols even though they aren’t.
- The relation detection is very simple, so it may miss more subtle claims or negative statements (e.g., “not associated with”).
- The topic modeling depends heavily on TF-IDF vocabulary, so some topics may blend together or be split by accident.
- The dataset is small, which makes topics less stable and limits how much structure the model can find.

---

## 5. Improvements / Next Steps
- Use **SciSpaCy**, which is trained on biomedical text, to get more accurate gene, disease detection.
- Combine rule-based and machine-learning methods to build a **hybrid NER system** that handles both easy and tricky cases.
- Use **dependency parsing** so the system can understand who relates to whom (e.g., variant → gene → disease) instead of just matching keywords.
- Do an **error analysis** to understand what the system is getting wrong and fix the most common mistakes.
- Try more advanced topic modeling methods like **BERTopic** to get cleaner, more meaningful clusters.



