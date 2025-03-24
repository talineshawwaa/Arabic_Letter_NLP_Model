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

## Step 3: Feature Extraction

To train a model that can understand Arabic letter morphology, we extracted both handcrafted and learned features from the cleaned dataset. These features capture the visual, positional, and contextual properties of each letter within its lemma.

---

### A. Positional Shape-Based Features

Each Arabic letter varies in shape depending on its position within a word — isolated, initial, medial, or final. We extracted features that describe:

- **Letter**: The normalized Arabic letter.
- **Character Index**: The position of the letter within its word.
- **Letter Position**: A label indicating whether the letter appears at the beginning, middle, end, or as a standalone character.
- **Shape Variants**: A list of Unicode representations for how the letter appears in different positions.
- **Number of Shape Variants**: Indicates morphological complexity of the letter.
- **Base Unicode**: Unicode of the primary shape (used as an additional signal).

We also one-hot encoded the letter positions (`pos_initial`, `pos_medial`, `pos_final`, `pos_isolated`) to make them machine-readable.

---

### B. Embedding-Based Features

To capture deeper semantic and contextual relationships, we extracted learned vector representations (embeddings) for each letter using two strategies:

1. **Custom Letter-Level FastText Embeddings**  
   We trained a FastText model directly on character sequences from our dataset. Each word was treated as a list of characters, and FastText learned sub-character co-occurrence patterns. This resulted in a 50-dimensional vector for each Arabic letter that captures its contextual usage.

2. **Word-Level Embeddings with AraBERT (optional)**  
   For more advanced models, we also extracted lemma-level contextual embeddings using the pretrained AraBERT model. These embeddings represent the entire word's semantic content and can help enhance performance in downstream NLP tasks.

---

### Final Feature Set

The final character-level dataset contains:

- `lemma_id`, `lemma`, `char_index`, `letter`
- Encoded position features (e.g., `pos_medial`)
- Morphological shape features (e.g., number of shape variants)
- Numerical identifiers (e.g., `letter_id`, `base_shape_unicode`)
- **Custom letter embeddings** from FastText (e.g., `embedding_0`, ..., `embedding_49`)

This structured dataset (`final_char_features.csv`) is now ready for model training, analysis, or further processing.

The final dataset with data cleaning, preprocessing and feature extraction can be found [here](https://drive.google.com/drive/folders/1xy5vNc_Hw3x796Qd2hBBd4CoP4g7rMtz?usp=sharing)
---
