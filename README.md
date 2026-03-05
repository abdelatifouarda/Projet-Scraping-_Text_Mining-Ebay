# Projet-Scraping-_Text_Mining-Ebay
# Laptop Listings Scraping & Text Similarity Analysis

## Project Overview

This project combines dynamic web scraping and text mining techniques to analyze laptop listings from an online marketplace.

The main objectives are:

- Extract structured product data using Selenium
- Clean and preprocess textual information
- Transform text into numerical representations using TF-IDF
- Visually explore the most informative terms using a TF-IDF-based Word Cloud
- Compute cosine similarity between listings
- Identify highly similar or potentially duplicate products
- Generate analytical insights from textual similarity patterns

This project demonstrates an end-to-end workflow from data extraction to analytical interpretation.

---

## Technologies Used

- Python
- Selenium (Dynamic Web Scraping)
- Pandas
- NumPy
- Scikit-learn
- Matplotlib
- WordCloud

---

## Data Collection

Laptop listings were scraped dynamically using Selenium due to JavaScript-rendered content.

Extracted fields include:

- Product Title
- Price
- Description
- Product URL
- Comments (when available)

The scraping phase focuses on collecting structured product information for downstream text analysis.

---

## Data Cleaning & Preparation

The following preprocessing steps were applied:

- Price normalization (removal of currency symbols and conversion to numeric format)
- Handling missing values
- Removing duplicate product titles
- Text normalization (lowercasing and punctuation removal)
- Merging title and description into a unified text column for analysis

These steps ensure data consistency before applying vectorization techniques.

---

## Text Vectorization (TF-IDF)

Text data was transformed into numerical vectors using TF-IDF with:

- Bi-grams (ngram_range=(1,2))
- Maximum feature limit
- English stopword removal
- Minimum document frequency filtering

Using bi-grams improves detection of compound technical phrases such as:

- "16gb ram"
- "512gb ssd"
- "intel core i7"

TF-IDF helps identify terms that are not only frequent, but also informative across listings.

---

## TF-IDF Word Cloud

To visually interpret the most informative terms, a Word Cloud was generated using TF-IDF scores (not raw word frequency).

This visualization highlights dominant technical attributes across listings, particularly hardware-related specifications such as RAM size, SSD capacity, and processor models.

The Word Cloud serves as a visual validation of the TF-IDF results, confirming that laptop listings are primarily differentiated by technical specifications rather than marketing language.

---

## Similarity Analysis

Cosine similarity was computed between all laptop listings based on their TF-IDF vectors.

Instead of using a fixed arbitrary threshold, the 95th percentile of similarity scores was selected to identify statistically significant similarity pairs.

This approach ensures that only unusually similar listings are flagged.

---

## Visualization

The project includes the following analytical visualizations:

- TF-IDF-based Word Cloud
- Distribution of similarity scores (histogram)
- Top most similar product pairs (horizontal bar chart)

The histogram provides a global overview of similarity patterns across all listings, while the bar chart highlights the strongest similarity relationships.

---

## Key Insights

- Laptop listings exhibit strong structural similarity due to standardized hardware terminology.
- Technical specifications (RAM, SSD, CPU models) dominate textual differentiation.
- High similarity clusters likely represent:
  - Duplicate postings
  - Resellers listing identical inventory
  - Standardized naming conventions

The analysis demonstrates how text mining techniques can uncover structural convergence within marketplace listings.

---

## Limitations

- Website structure may change due to dynamic DOM rendering.
- Similarity analysis is purely text-based (no image or metadata comparison).
- No clustering or classification models were applied.

---

## Future Improvements

- Add clustering techniques (e.g., KMeans)
- Integrate topic modeling (LDA)
- Incorporate price-based anomaly detection
- Build an interactive dashboard using Streamlit

---

## Author

Data Analyst focused on Web Scraping & Text Mining.
