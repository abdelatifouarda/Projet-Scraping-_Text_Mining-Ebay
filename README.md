# eBay Laptop Listings: Web Scraping & TF-IDF Text Similarity Analysis

## Executive Summary

This project builds an end-to-end pipeline that scrapes laptop listings from eBay France, cleans and vectorizes the textual content using TF-IDF, and applies cosine similarity to detect near-duplicate or highly overlapping listings. The analysis shows that laptop listings are differentiated almost entirely by standardized technical vocabulary (CPU, RAM, SSD) rather than marketing language, and that a small but meaningful subset of listings are near-duplicates of one another.

## Business Problem

Online marketplaces host thousands of listings for the same product category, often posted by multiple resellers using near-identical titles and descriptions. This makes it difficult for buyers to compare genuinely distinct offers and for the platform to maintain a clean, non-redundant catalog. This project investigates whether unsupervised text-mining techniques can automatically surface these near-duplicate listings from raw scraped data, without relying on manual review or a labeled dataset.

## Business Objectives

- Collect real-world, unstructured marketplace data through dynamic web scraping.
- Convert unstructured product text (titles, descriptions, buyer comments) into a structured, analyzable format.
- Quantify how similar listings are to one another using an interpretable, unsupervised method (TF-IDF + cosine similarity).
- Surface listings that are unusually similar, as a proxy for duplicate postings or repeated reseller inventory.

## Dataset

The dataset was collected directly from eBay France (`ebay.fr`) search results for the query **"laptop"**, using Selenium to render JavaScript-based content. For each listing, the following fields were extracted:

| Field | Description |
|---|---|
| `idProduits` | Sequential listing identifier |
| `titles` | Product title |
| `price` | Listing price (originally in EUR, as text) |
| `urls` | Direct link to the listing |
| `description` | Full item description (scraped from the listing page) |
| `commentaire` | Buyer feedback comments, when available |

**Note on scope:** the scraper collects listings from a single search-results page (no pagination loop), so the dataset represents one page of "laptop" results rather than the full eBay catalog for that query.

## Project Workflow

1. **Dynamic Web Scraping (Selenium):** Launch a Chrome session, search "laptop" on eBay France, and extract title, price, and URL for each listing card on the results page.
2. **Detail-Page Enrichment:** Visit each listing's URL individually to scrape its full description and any buyer comments.
3. **Data Export:** Persist the combined results to a CSV file (`resultatsEbay.csv`).
4. **Data Cleaning:**
   - Handle missing titles/comments by filling with placeholder values.
   - Normalize price strings (strip the `EUR` currency label, convert comma decimals to dot decimals, coerce to numeric) and impute remaining missing prices with the median price.
   - Remove duplicate listings based on exact title matches.
5. **Text Preprocessing:** Lowercase text, strip digits and punctuation, remove English and French stopwords, and lemmatize the remaining tokens (title and comment fields are processed separately, then merged into a single `all_text` column).
6. **Vectorization:** Represent listings as a Bag-of-Words matrix, then as a TF-IDF matrix using unigrams and bigrams (`ngram_range=(1,2)`), with document-frequency filtering (`min_df=2`, `max_df=0.8`) and a 3,000-feature cap.
7. **TF-IDF Word Cloud:** Visualize the most informative terms across all listings, weighted by their aggregated TF-IDF scores rather than raw frequency.
8. **Similarity Analysis:** Compute pairwise cosine similarity across all TF-IDF vectors, inspect the overall distribution of similarity scores, and flag pairs above a **fixed threshold of 0.90** as likely near-duplicates.
9. **Visualization of Results:** Plot the distribution of all pairwise similarity scores and a horizontal bar chart of the top 15 most similar listing pairs.

## Technologies Used

- **Python**
- **Selenium** + **webdriver-manager** (dynamic web scraping)
- **Pandas**, **NumPy** (data handling)
- **NLTK** (stopword removal, lemmatization)
- **Scikit-learn** (`CountVectorizer`, `TfidfVectorizer`, `cosine_similarity`)
- **Matplotlib**, **WordCloud** (visualization)

## Results

- The pairwise similarity distribution shows that **most listing pairs fall between 0.0 and 0.2 similarity**, meaning the majority of scraped listings are textually distinct from one another.
- A **small cluster of pairs scores above 0.9**, indicating a subset of listings that are near-identical in wording.
- The TF-IDF word cloud is dominated by two term categories: seller-confidence language (e.g., "great," "good") and hardware specifications (e.g., "Intel," "GB," "RAM").
- Listings flagged as near-duplicates (similarity > 0.90) share nearly identical hardware specifications, often across different brands — suggesting the model captures functional/spec-based equivalence rather than brand identity.

## Business Insights

- Laptop listings on the platform are structurally standardized: buyers and sellers converge on the same technical vocabulary (RAM size, storage capacity, processor model) far more than on differentiated marketing copy.
- A meaningful minority of listings are near-duplicates of each other, consistent with resellers posting the same or very similar inventory multiple times.
- Because differentiation is spec-driven rather than brand-driven, listings from different brands with matching specs can appear as substitutes for one another in this analysis.

## Business Recommendations

- A marketplace could use this type of similarity scoring as a first-pass, automated flag for potential duplicate or reseller-repeated listings, reducing catalog clutter and improving buyer trust.
- Sellers aiming to stand out should be encouraged to add more differentiated, non-boilerplate descriptive content, since generic spec-only listings are the hardest to tell apart.
- Before using a similarity threshold in production, it should be validated against a larger, multi-page sample and compared against a data-driven cutoff (e.g., a percentile of the score distribution) rather than a single fixed value, since the right threshold likely varies with sample size and search category.

## Author

Data Analyst focused on Web Scraping & Text Mining.
