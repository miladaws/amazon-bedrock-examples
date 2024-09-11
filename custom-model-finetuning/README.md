# Fine-Tuning and Deploying Custom Models on Amazon Bedrock

## Overview

This project provides a step-by-step guide to fine-tuning Cohere's `command-light-text-v14` model using **Amazon Bedrock**. The use case focuses on creating a medical summarizer by fine-tuning the model with biomedical abstracts from PubMed. The project covers everything from setting up AWS resources to deploying the fine-tuned model for inference.

## Prerequisites

Before you start, ensure you have the following prerequisites:
- **AWS Account**: Access to Amazon Bedrock and S3.
- **AWS CLI**: Installed and configured for your environment.
- **Python Environment**: Ensure you have Python 3.7+ installed.
- **Required Libraries**: Install the dependencies using `pip`:
  ```bash
  pip install boto3 pandas requests
  ```

## Project Structure

```
.
├── README.md                   # Project documentation (this file)
├── dataset/                    # Directory where fetched datasets are stored
│   └── pubmed_abstracts.jsonl   # Fetched PubMed abstracts in JSONL format
├── notebook.ipynb               # Jupyter notebook with the complete workflow
└── scripts/                     # Python scripts used in the notebook
```

## Workflow

1. **Fetch Biomedical Data**
   - The notebook fetches biomedical abstracts related to a specific query (e.g., `diabetes`) using the Entrez E-utilities from PubMed.
   - These abstracts are saved as a JSONL file, where each entry contains a `prompt` (the abstract text) and a placeholder `completion`.

2. **Save Dataset to S3**
   - The collected dataset is uploaded to an S3 bucket that will be used during the fine-tuning process.

3. **Create an IAM Role**
   - An IAM role is created with appropriate permissions, allowing Amazon Bedrock to access the S3 bucket for model fine-tuning.

4. **Fine-Tune the Model**
   - The notebook fine-tunes Cohere's `command-light-text-v14` model using the uploaded dataset. It uses a custom job name and configuration parameters like `batchSize`, `learningRate`, and `epochCount`.

5. **Deploy and Inference**
   - Once the fine-tuning process is complete, the custom model is used for inference. You can pass a medical abstract, and the model will return a summarized version of the text.

## Usage

### Running the Notebook

1. **Clone the repository**:
   ```bash
   git clone https://github.com/yourusername/bedrock-finetuning-project.git
   cd bedrock-finetuning-project
   ```

2. **Open the notebook**:
   You can run the Jupyter notebook by using the following command:
   ```bash
   jupyter notebook notebook.ipynb
   ```

3. **Follow the steps** in the notebook to fine-tune the model. Be sure to set your own AWS credentials and region.

### Example Inference

Once the fine-tuned model is deployed, you can test it by passing medical abstracts like the example below:

```python
prompt = """
Summarize the following medical abstract:
This study investigates the impact of diabetes on cardiovascular health. The research involved a cohort of 200 patients 
with type 2 diabetes, tracking their cardiovascular events over a period of 5 years. The results indicate a significant 
correlation between diabetes duration and the incidence of heart disease.
"""
```

### Expected Output:
The model should return a concise summary of the abstract.

## Configuration

- **AWS Region**: Ensure you're working in a region that supports Amazon Bedrock, such as `us-east-1`.
- **S3 Bucket**: Update the S3 bucket name and paths in the notebook to match your environment.

## Notes

- This project is focused on fine-tuning a text model for biomedical applications, but the same approach can be applied to other domains.
- Make sure to clean up your AWS resources (IAM roles, S3 buckets) to avoid unnecessary charges.

## License

This project is licensed under the MIT License.
