# Online Retail Item Similarity Finder

Welcome to the **Online Retail Item Similarity Finder**!  
This project uses machine learning and natural language processing to find similar items in an online retail dataset based on product descriptions, stock codes, and country information.

## 🚀 Features

- Cleans and preprocesses retail data
- Combines multiple features into a single text representation
- Uses `CountVectorizer` and `cosine_similarity` to find similar items
- Returns the top 5 most similar products for any given item

## 📦 Dataset

The project uses `online_retail_II.xlsx`.  
Make sure this file is present in your project folder.

## 🛠️ How It Works

1. **Data Loading:**  
   Loads the Excel file using pandas.

2. **Preprocessing:**  
   - Selects relevant columns (`StockCode`, `Description`, `Country`)
   - Cleans text (lowercase, replaces spaces with underscores)
   - Combines features into a single string

3. **Vectorization:**  
   Converts combined features into vectors using `CountVectorizer`.

4. **Similarity Calculation:**  
   Calculates cosine similarity between all items.

5. **Recommendation:**  
   For a given item description, finds and displays the top 5 most similar items.

## 📝 Example Usage

```python
import pandas as pd
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.metrics.pairwise import cosine_similarity

# Load data
org_df = pd.read_excel('online_retail_II.xlsx')
df = org_df[["StockCode", "Description", "Country"]].copy()
df["Country"] = df["Country"].str.replace(" ", "_").str.lower()
df["Description"] = df["Description"].str.replace(" ", "_").str.lower()
df["MixedFeatures"] = df["StockCode"].astype(str) + " " + df["Description"] + " " + df["Country"]

# Vectorize
vectorizer = CountVectorizer(stop_words="english")
df["MixedFeatures"] = df["MixedFeatures"].fillna("")
vector = vectorizer.fit_transform(df["MixedFeatures"])

# Cosine similarity
similarities = cosine_similarity(vector)
description = "15cm_christmas_glass_ball_20_lights"
item_index = df[df["Description"] == description].index[0]
item_similarities = similarities[item_index]

item_df = pd.DataFrame({
    "Description": df["Description"],
    "Similarity": item_similarities
})
item_df = item_df[item_df.index != item_index].sort_values("Similarity", ascending=False).head(5)
print(item_df)
```
## 🖥️ Requirements

- Python 3.x
- pandas
- scikit-learn
- openpyxl

Install requirements with:
```
pip install pandas scikit-learn openpyxl
```
