# Yelp-Reviews-Sentiment-Data-Analytics-Pipeline

An end-to-end big data analytics project using Python, Snowflake, AWS S3, and SQL to analyze and extract actionable insights from over 7 million Yelp review records.

## Project Overview

This project demonstrates a scalable cloud-based data pipeline for processing massive, semi-structured JSON datasets. It includes data engineering, cloud data warehousing, real-time sentiment analysis using Python UDFs, and advanced analytics using SQL.

<p align="center">
  <img src="project_architecture.png" width="600" alt="Project Architecture">
</p>

## Key Features

- **Big Data Processing:** Splits a 5GB raw Yelp reviews JSON file (~7M records) into manageable chunks using Python.
- **Cloud Data Storage:** Uploads split files to AWS S3 for scalable, fault-tolerant storage.
- **Cloud Data Warehouse:** Loads raw JSON into Snowflake, flattens variant fields, and builds structured tables for analysis.
- **Sentiment Analysis:** Implements a custom Python UDF in Snowflake (using TextBlob) for automated classification of review sentiment (Positive, Neutral, Negative).
- **Advanced SQL Analytics:** Surfaces trends by business, city, and category to drive marketing, product, and customer experience decisions.

## Pipeline Steps

1. **Data Preparation (Python):**
   - Split large Yelp reviews JSON into smaller files for efficient upload and parallel processing.
2. **Data Ingestion:**
   - Upload split JSON files to AWS S3.
   - Ingest files into Snowflake using Snowflake’s COPY INTO and JSON variant handling.
3. **Data Modeling (Snowflake SQL):**
   - Flatten and extract key review and business attributes into structured tables.
   - Join review data with business metadata for enriched analytics.
4. **Sentiment Analysis (Snowflake Python UDF):**
   - Deploy a Python UDF using TextBlob for on-the-fly sentiment scoring of reviews.
5. **Data Analysis (SQL):**
   - Aggregate and visualize sentiment trends, star ratings, and business/category performance.
   - Identify actionable insights for customer experience and marketing strategies.

## Example SQL Queries

```sql
-- Run sentiment analysis on all reviews
SELECT review_id, business_id, analyze_sentiment(review_text) AS sentiment
FROM tbl_yelp_reviews
LIMIT 100;

-- Find top cities by average sentiment
SELECT city, AVG(CASE WHEN sentiment = 'Positive' THEN 1 ELSE 0 END) AS pct_positive
FROM tbl_yelp_reviews
JOIN tbl_yelp_businesses USING (business_id)
GROUP BY city
ORDER BY pct_positive DESC
LIMIT 10;
```


## Tech Stack
**Python**: Data preparation and JSON file splitting

**AWS S3**: Cloud storage for raw and processed data

**Snowflake**: Cloud data warehouse, ETL, SQL analytics

**TextBlob**: Python sentiment analysis library (in Snowflake UDF)

**SQL**: Data modeling and advanced analytics

## Results & Insights
Processed and modeled over 7 million reviews and 100K businesses with scalable, production-grade ETL.

Automated sentiment classification of reviews at scale.

Surfaced business and city-specific insights, enabling data-driven decision-making for marketing and customer engagement.

## How to Run
Split the raw JSON file using Python:
(see split_json.py script for details)

Upload split files to AWS S3.

## In Snowflake:

Create external tables for JSON variant data.

Transform and model tables as shown in the provided SQL scripts.

Register and use the analyze_sentiment Python UDF.

Query and analyze data with SQL.

## License
MIT License
