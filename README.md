# E-commerce customer segmentation and recommendations

A notebook on the transactions of an online retailer (the *Online Retail* dataset: 541,909 invoice lines from December 2010 to
December 2011, 4,372 customers with an identifier). It looks at sales trends, scores customers by recency, frequency and monetary value
(RFM) and recommends products from the similarity between customers.

## What the notebook does

1. **Cleaning:** removes the rows without a customer id, which leaves 4,372 customers.
2. **Business trends:** sales and invoices by day of the week, month and hour, and the monthly revenue.
3. **RFM scoring:** for each customer, the days since the last purchase (recency), the number of invoice lines (frequency) and the
   total spent (monetary). Each is turned into a rank score from 0 to 100 and they are combined as `0.2 R + 0.3 F + 0.5 M`. Loyalty
   tiers are defined from thresholds on that score, and **K-Means** is run on it (the elbow suggests 3 groups; silhouette 0.59).
4. **Recommender (user-based collaborative filtering):** a customer-by-product matrix (bought or not) and the cosine similarity between
   customers. For a customer, it recommends the products bought by the most similar customer that they have not bought yet. The example
   is customer 12347, whose most similar customer (17396, similarity 0.30) leads to 9 recommended products.
5. **A separate practice exercise:** a content-based movie recommender on a dataset of 4,806 movies, unrelated to the retail data.

## Known limitations

This notebook is a learning project from the Learnbay program. These are the limits I know about:

- **K-Means runs on a single number.** The input is the combined RFM score (one column), so the groups are slices of that score and
  come out almost the same size (1,553 / 1,443 / 1,376 customers). The silhouette of 0.59 describes how the score is distributed,
  not natural groups of customers.
- **The weights 0.2 / 0.3 / 0.5 are arbitrary**, and the rank scores discard how far apart customers really are (a customer who spent
  100 times more gets only a slightly higher score).
- **Frequency counts invoice lines**, not distinct invoices.
- **The recommender is not evaluated:** there is one worked example and no held-out test. Its output also includes codes that are not
  products (for example `M`, a manual entry).
- The movie recommender has nothing to do with the retail data.

## What I plan to change

- RFM in **three dimensions**, on a logarithmic scale and standardised, instead of one weighted score.
- Choose the number of segments with **silhouette and stability checks**, and **profile** each segment (who they are, what they buy).
- **Evaluate the recommender** on purchases held out in time (does it recommend what the customer later bought?), against a baseline of
  "the most popular products".

## Data

The data file is not stored in this repository. The notebook reads `online_shopping.xlsx`, which is the *Online Retail* dataset
(UCI Machine Learning Repository; also on Kaggle). The movie exercise reads `movie_dataset.csv`.

## Run it

```bash
pip install pandas numpy matplotlib seaborn scikit-learn yellowbrick openpyxl jupyter
jupyter notebook PROJECT_E_commerce.ipynb
```
