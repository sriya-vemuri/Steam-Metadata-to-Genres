The data file used was https://www.kaggle.com/datasets/vicentearce/steam-and-steam-spy-raw-datasets
# Steam Genre Prediction

A multi-label machine learning project that predicts Steam game genres from early-stage app metadata. This project was built as a data-driven “Genre Tagging Assistant” to help game studios and publishers choose better genre tags before launch, improve game discovery, and support stronger positioning and distribution decisions.

## Overview

Choosing genre tags for a new game affects discoverability, targeting, bundles, and marketing. In practice, those decisions are often based on intuition or quick competitor checks, which can miss useful secondary genres and create inconsistent tagging.

This project builds a predictive system that uses early game metadata such as platform availability, pricing, screenshots, trailers, demos, and developer/publisher details to recommend likely genre tags with confidence scores. The goal is to make tagging faster, more consistent, and more data-driven.

## Business Problem

Steam is a large PC game distribution platform with tens of thousands of listed games. When developers prepare to launch a title, selecting the right genre tags is critical because those tags influence:

- player discovery
- ad targeting
- bundle fit
- influencer outreach
- store positioning

A data mining solution helps reduce guesswork by recommending a ranked list of likely genres and surfacing the metadata features that matter most for those recommendations.

## Dataset

This project uses a merged Steam dataset from Kaggle containing 66,928 games and 31 variables, with one row per app.

Key variables include:

- `steam_appid`
- `name`
- `genres`
- `price_overview`
- `is_free`
- `required_age`
- `platforms`
- `supported_languages`
- `developers`
- `publishers`
- `screenshots`
- `movies`

The raw data contained missing values, mixed data types, malformed text, and semi-structured fields, so substantial cleaning and parsing were required before modeling.

## Project Workflow

### 1. Data Cleaning and Preparation

The dataset was cleaned using Python and Pandas by:

- removing redundant and mostly empty columns
- standardizing column names
- cleaning text fields
- stripping HTML tags and special characters
- parsing JSON-like columns such as genres, categories, and developers
- converting fields into numeric, date, and boolean formats

This produced a cleaner structured dataset suitable for analysis and modeling.

### 2. Exploratory Data Analysis

Several analyses were performed to understand the Steam marketplace and genre patterns:

- free vs paid distribution analysis
- review text cleaning and word cloud generation
- genre frequency analysis
- genre sentiment analysis using VADER
- co-occurrence heatmaps for overlapping genres
- genre popularity vs sentiment comparisons

Key observations included:

- only about 11% of games were free, meaning paid titles dominate the platform
- narrative and gameplay-related words such as “fun,” “experience,” and “character” appeared frequently in reviews
- “indie,” “action,” “adventure,” and “casual” were among the most common genres
- “action” and “indie” were the strongest genre co-occurrence pair
- “adventure” and “strategy” were among the most positively perceived genres

### 3. Modeling

The genre prediction task was framed as a multi-label classification problem because a game can belong to multiple genres.

Three models were built and compared:

- Logistic Regression
- Random Forest
- Linear Support Vector Classifier (Linear SVC)

These models were selected because they are interpretable, efficient on structured tabular data, and practical for a business-facing recommendation tool.

More complex approaches such as gradient boosting and deep learning were considered, but not prioritized because they would increase complexity and reduce transparency.

## Evaluation

The models were evaluated using 10-fold cross-validation.

Metrics used:

- Micro-F1
- Macro-F1
- Weighted-F1
- Hamming Loss
- tuned prediction threshold

### Best Performing Model

Random Forest performed best overall for multi-label tagging:

- Micro-F1: `0.5203 ± 0.0285`
- Macro-F1: `0.1661 ± 0.0174`
- Weighted-F1: `0.5093 ± 0.0318`
- Hamming Loss: `0.1317 ± 0.0099`
- Tuned Threshold: `0.3300 ± 0.0350`

### Other Models

Logistic Regression:

- Micro-F1: `0.3953 ± 0.0153`
- Macro-F1: `0.2015 ± 0.0134`
- Weighted-F1: `0.5470 ± 0.0190`
- Hamming Loss: `0.3012 ± 0.0266`

Linear SVC:

- Micro-F1: `0.3902 ± 0.0215`
- Macro-F1: `0.2043 ± 0.0144`
- Weighted-F1: `0.5502 ± 0.0234`
- Hamming Loss: `0.3088 ± 0.0445`

### Interpretation

- Random Forest was the strongest overall tagger based on Micro-F1 and lowest Hamming Loss
- Macro-F1 remained low across all models because rare genres suffered from strong class imbalance
- Weighted-F1 was higher for Logistic Regression and Linear SVC because common labels dominated the weighting

## Business Impact

The final solution can be deployed as a Genre Tagging Assistant for game studios and publishers.

A user can input early game details such as:

- price
- platform
- media availability
- launch metadata

The tool then returns:

- a ranked list of likely genres
- confidence scores
- a transparent basis for the recommendation

This helps:

- reduce manual effort
- improve tagging consistency
- speed up store setup
- surface secondary genres for better reach
- improve game discoverability and positioning

## Deployment Considerations

Before production use, several risks should be considered:

- model accuracy depends on the quality and completeness of input data
- missing values such as screenshots or pricing can reduce prediction quality
- the model may become outdated as genre trends shift over time
- common genres like Action and Indie may be over-favored due to class imbalance
- human review should remain part of the workflow

Recommended safeguards:

- retrain the model regularly with fresh Steam data
- keep a human in the loop for final tag approval
- monitor genre imbalance and rebalance training data where possible
- show feature-level explanations to improve trust and usability

## Tech Stack

- Python
- Pandas
- scikit-learn
- VADER Sentiment Analyzer
- data visualization libraries for EDA
- cross-validation and multi-label classification workflows

## Use Case

This project is best suited for:

- game studios preparing store pages
- publishing teams optimizing game positioning
- marketing teams testing discoverability strategies
- product teams evaluating likely genre fit before launch

## Key Takeaways

- early-stage metadata can support meaningful genre recommendations
- interpretable ML models can provide both predictions and business insight
- Random Forest offered the best overall tagging performance
- class imbalance remains a major challenge for rare genre prediction
- a human-reviewed decision support tool is the most practical deployment approach

## Team

Team 44:

- Sriya Vemuri
- Amna Mahmood
- Samir Dar
- Kerry Chen
- Calida Mathias

## Future Improvements

Potential next steps include:

- handling class imbalance more aggressively
- testing gradient boosting methods such as XGBoost or LightGBM
- incorporating text embeddings for richer metadata understanding
- improving rare genre recall
- building a lightweight UI for real-time genre suggestions
- adding explainability visuals for feature importance and confidence
