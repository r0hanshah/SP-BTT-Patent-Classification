# SP-BTT-Patent-Classification

## Overview
This project classifies patent abstracts into relevant categories using large language models (LLMs) for financial insights. It leverages few-shot learning examples and keyword matching to refine predictions and provides functionalities for creating balanced datasets for training, validation, and testing.

The repository contains code to process, filter, and classify patent abstracts from a public dataset, enabling the identification of patents relevant to various financial and technological industries.

---

## Features
- **Patent Dataset Handling**: Loads, filters, and processes the public patents dataset.
- **Few-Shot Learning**: Incorporates few-shot examples to enhance classification prompts.
- **Custom Industry Classification**: Classifies patents based on industry-specific keywords.
- **Balanced Dataset Creation**: Generates datasets with equal positive and negative samples.
- **Reviewer Allocation**: Prepares review tables for manual verification and expert review.

---

## Installation

### Prerequisites
1. **Python**: Version 3.8 or above.
2. **Google Colab or Jupyter Notebook**: For running the notebook.
3. Install the following Python libraries:
   ```
   pip install datasets google-generativeai pandas numpy matplotlib
   ```

---

## Folder Structure
- **`required_files`**:
  - Contains all the files required for the project:
    - `patent_classification.json`: Few-shot examples for classification.
    - `financial_topics.txt`: List of financial topics and associated keywords.
    - Any other supporting files used in the notebook.

- **`notebooks`**:
  - Contains the main Jupyter Notebook (`code_pipeline.ipynb`) to execute the pipeline.

- **`output_files`**:
  - Where classified datasets and review tables are saved.

---

## Usage

### Step 1: Update File Paths
Ensure that file paths in `code_pipeline.ipynb` point to the `required_files` folder. For example:
```python
file_path = 'required_files/financial_topics.txt'
```

### Step 2: Run the Notebook
Open `code_pipeline.ipynb` in Jupyter Notebook or Google Colab and execute the cells sequentially.

### Step 3: Configure the API
Set up your Google Generative AI API key in the notebook:
```python
from google.colab import userdata
GOOGLE_API_KEY = userdata.get('GOOGLE_API_KEY')
```

### Step 4: Specify Industry
Update the `industry` variable in the notebook with the desired category:
```python
industry = "Autonomous Vehicles"
```

### Step 5: Generate Classifications
Execute the `classify_patents` function to classify patent abstracts:
```python
classified_data = classify_patents(total_abstracts_df_sets, industry, keywords, few_shot_examples)
```

### Step 6: Save Results
Use the `save_balanced_csv` function to create a balanced dataset:
```python
balanced_data = save_balanced_csv(classified_data, "output_files/balanced_dataset.csv", industry)
```
Generate a review table for manual validation:
```python
review_table = prepare_review_table(classified_data)
review_table.to_csv('output_files/classified_review_set.csv', index=False)
```

---

## Files in `required_files`
- **`patent_classification.json`**: Contains few-shot examples categorized by industries.
- **`financial_topics.txt`**: A list of financial topics and their associated keywords.
- **`public_patents_dataset.pkl`**: Pickled dataset of processed patent abstracts.

---

## Key Functions

### `generate_prompt`
Generates a prompt for the LLM using few-shot examples and keywords.

### `predict_industry`
Uses the LLM to classify a patent abstract as relevant (`Yes`) or not (`No`) for a given industry.

### `classify_patents`
Processes the entire dataset, filters by keywords, and classifies abstracts into positive and negative categories.

### `save_balanced_csv`
Creates a balanced dataset with equal positive and negative samples.

### `prepare_review_table`
Prepares a table for manual review, assigning reviewers and adding space for comments.

---

## Outputs
- **Balanced Dataset**:
  - Saved in `output_files/balanced_dataset.csv`.
- **Classified Review Table**:
  - Saved in `output_files/classified_review_set.csv`.

---

## Customization
To classify patents for a different industry:
1. Update the `industry` variable with the new category name.
2. Add relevant keywords to the `industry_keywords` dictionary in the notebook.
3. Ensure that `patent_classification.json` includes few-shot examples for the chosen industry.

---

## Limitations
- API rate limits may slow processing; adjust the `rpm` parameter accordingly.
- Manual keyword updates are needed for new industries.
