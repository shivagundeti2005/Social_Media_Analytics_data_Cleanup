# Social_Media_Analytics_data_Cleanup
🚀 Omnichannel Social Media Analytics Data Cleaning Pipeline

📌 Project Overview
This project focuses on cleaning, standardizing, and integrating social media analytics data from Meta, X (Twitter), LinkedIn, and TikTok into a single analytics-ready dataset. Using Python, Pandas, and NumPy, the pipeline resolves schema inconsistencies, normalizes timezones, removes invalid records, and produces a unified source of truth for cross-platform analysis and reporting.

📊 Data Transformation After Cleaning
Before Cleaning

The raw datasets contained:

Different column names for identical metrics
Mixed timezone formats (UTC, IST)
Internal test account activity
Missing engagement metrics after merging
Platform-specific schema structures

After Cleaning
https://github.com/shivagundeti2005/Social_Media_Analytics_data_Cleanup/blob/main/cleaned_csv%20(1).png

Standardized Metrics
Original Platform Metrics	Standardized Metric
likes, favourites	likes
views, video_views	impressions
retweets	shares
unique_views	reach
comments	comments

⚠️ The Challenge

The project presented several real-world data engineering challenges:

1. Fragmented Schemas

Each platform used proprietary naming conventions for the same business metrics.

Example:

Meta	Twitter	LinkedIn	TikTok
likes	favourites	likes	likes
impressions	views	impressions	video_views

This prevented direct comparison across platforms.

2. Timezone Ambiguity

The post_date column contained multiple timezone representations:

2024-01-01 10:30:00 UTC
2024-01-01 16:00:00 IST

Timezone abbreviations such as IST are ambiguous and cannot be reliably parsed without explicit mapping.

3. Polluted Data

Internal testing accounts such as:

TEST001
INTERNAL02

were mixed with production data, creating inaccurate engagement metrics.

4. Structural Missing Values

When combining four different schemas, some platforms lacked specific metrics entirely, introducing structural NaN values across the merged dataset.

🔄 The Solution & Pipeline
Phase 1 — Data Ingestion
pd.read_csv(file, skipinitialspace=True)
Loaded all CSV files
Removed parsing issues caused by extra spaces
Standardized column formatting
Phase 2 — Schema Standardization

Mapped platform-specific fields into a common business model.

video_views → impressions
favourites → likes
retweets → shares

Result:

post_id
account_id
post_date
impressions
reach
likes
comments
shares
Phase 3 — Data Integration

All cleaned datasets were vertically concatenated into a single dataframe.

pd.concat(datasets, ignore_index=True)

This produced a unified omnichannel dataset.

Phase 4 — Temporal Normalization

Explicit timezone mapping was applied:

{
    "UTC": "+00:00",
    "IST": "+05:30"
}

All timestamps were converted to:

datetime64[ns, UTC]

ensuring consistent time-series analysis.

Phase 5 — Data Quality & Cleaning
Remove Internal Accounts
df = df[~df["account_id"].isin(test_accounts)]
Handle Missing Metrics
df[metric_cols] = df[metric_cols].fillna(0)
Convert to Integer
df[metric_cols] = df[metric_cols].astype(int)
Sort Chronologically
df.sort_values("post_date")

📈 Results
Dataset Quality Improvements

✅ Unified 4 independent social media datasets
✅ Standardized engagement metrics across platforms
✅ Achieved 100% UTC timestamp conformity
✅ Removed internal test account contamination
✅ Eliminated structural missing-value issues
✅ Produced a clean, analysis-ready dataset

Final Dataset Summary
Metric	Result
Platforms Integrated	4
Unified Schema	Yes
Timezone Standardized	UTC
Missing Metrics Handled	Yes
Test Accounts Removed	Yes
Output Format	CSV
Ready for BI/Dashboarding	Yes

🛠️ Tech Stack
Programming Language
Python
Libraries
Pandas
NumPy
Development Environment
Jupyter Notebook
VS Code
Data Format
CSV
Version Control
Git
GitHub

📂 Project Structure
├── 08a_meta_analytics.csv
├── 08b_twitter_analytics.csv
├── 08c_linkedin_analytics.csv
├── 08d_tiktok_analytics.csv
│
├── cleaning_pipeline.ipynb
├── cleaned_omnichannel_analytics.csv
├── cleaned_csv.png
│
├── requirements.txt
├── environment.yml
└── README.md

🎯 Key Skills Demonstrated
Data Cleaning
Data Wrangling
Data Integration
ETL Development
Schema Standardization
Timezone Normalization
Data Quality Management
Pandas & NumPy
Analytical Dataset Preparation
Reproducible Data Pipelines
