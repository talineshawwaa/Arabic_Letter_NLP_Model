# Arabic_Letter_NLP_Model

## 📌 Objectives
The goal of this project is to enhance the ability of Natural Language Processing (NLP) models to understand the morphological complexity of the Arabic script, specifically the **shape variations of Arabic letters** depending on their **position in a word**.  
Arabic letters can take on different forms when they appear:
- **Isolated**
- **At the beginning of a word**
- **In the middle**
- **At the end**

This project builds a dataset that captures these variations and uses it to train a model that can classify or process each letter based on its contextual form. By doing so, we aim to improve the robustness of Arabic NLP tools and make them more accessible across the MENA region.

---

## Step 1: Data Collection

We used the **Qabas Lexicon** dataset, a rich lexicographic resource containing over **58,000 Arabic lemmas**, covering diverse parts of speech and dialectal usage. Each lemma is associated with metadata like:
- Root
- Part of Speech (`pos`)
- Grammatical Category (`pos_cat`)
- Gender
- Language type (Modern Standard, Dialectal)

This dataset was chosen because it provides a wide variety of typed Arabic words, which is crucial for covering different letter shape variations.

---

## Step 2: Data Cleaning and Preprocessing

We performed the following preprocessing steps using the code provided in the notebook:

1. **Column Selection**  
   Extracted relevant fields such as `lemma_id`, `lemma`, `language`, `pos_cat`, `pos`, `root`, and `gender`.

2. **Missing Value Handling**  
   Removed any entries where the `lemma` field was missing to ensure data quality.

3. **Text Normalization**  
   - Removed Arabic diacritics.
   - Normalized letter variations (e.g., converting `أ`, `إ`, `آ` to `ا` and `ى` to `ي`).

4. **Arabic Text Reshaping**  
   Used the `arabic_reshaper` and `bidi` libraries to correctly display and process Arabic text for right-to-left orientation.

5. **Tokenization**  
   Split each normalized lemma into individual Arabic characters for further analysis.

6. **Letter Shape Mapping**  
   For each character, we mapped it to its **four possible contextual forms** (isolated, initial, medial, and final) using a predefined dictionary of Arabic Unicode shape variants. This allowed us to associate each letter with how it could appear in a typed word depending on its position.

7. **Output**  
   The final cleaned DataFrame contains the original lemma, its normalized
