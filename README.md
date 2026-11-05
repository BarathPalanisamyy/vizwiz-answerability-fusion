# Flagging Unanswerable Visual Questions with Image-Text Fusion

## Project Overview

This project investigates whether combining image and question features
improves the detection of unanswerable visual questions.

The task is binary classification:
- 0: Answerable
- 1: Unanswerable

The implementation uses the VizWiz-VQA dataset and compares four models.

## Dataset

Dataset: VizWiz-VQA

Official website:
https://vizwiz.org/tasks-and-datasets/vqa/

Dataset splits:
- Training: 20,523 examples
- Validation: 4,319 examples
- Testing: 8,000 examples

The dataset contains images and natural-language questions
collected from people who are blind or have low vision.

## Feature Extraction

Two pretrained models are used as frozen feature extractors:

1. BERT (bert-base-uncased)
   - Extracts 768-dimensional question representations.

2. Vision Transformer (google/vit-base-patch16-224-in21k)
   - Extracts 768-dimensional image representations.

## Implemented Models

1. Question-only:
   Uses BERT question features with an MLP classifier.

2. Image-only:
   Uses ViT image features with an MLP classifier.

3. Early Fusion:
   Concatenates BERT and ViT features into a 1536-dimensional
   representation before classification.

4. Gated Fusion:
   Learns feature-wise weights to combine projected BERT
   and ViT representations.

## Training Configuration

- Framework: PyTorch
- Optimizer: AdamW
- Learning rate: 0.001
- Weight decay: 0.01
- Batch size: 256
- Maximum epochs: 30
- Early stopping patience: 4
- Dropout: 0.3
- Random seed: 42
- Model selection: Lowest validation cross-entropy loss

## Test Results

| Model | Accuracy | Macro-F1 | AUROC | AUPRC |
|---|---:|---:|---:|---:|
| Question-only | 0.7612 | 0.6241 | 0.7412 | 0.5187 |
| Image-only | 0.7804 | 0.6743 | 0.7720 | 0.5917 |
| Early Fusion | 0.8180 | 0.7564 | 0.8619 | 0.7165 |
| Gated Fusion | 0.8111 | 0.7373 | 0.8533 | 0.7005 |

Early Fusion achieved the strongest overall performance.

## Repository Structure

- vizwiz_project_2_2.ipynb: Main Colab notebook
- requirements.txt: Python dependencies
- models/: Trained classifier weights
- results/: Evaluation tables and figures

## Reproducing the Experiments

1. Clone this repository.
2. Install dependencies using:
   pip install -r requirements.txt
3. Download the VizWiz-VQA dataset from the official website.
4. Open vizwiz_project_2_2.ipynb in Google Colab.
5. Configure the Google Drive and dataset paths.
6. Run the notebook cells in order to extract features,
   train models, and reproduce evaluation results.

The notebook uses Google Drive for storing extracted features
and model outputs. Dataset images and extracted feature arrays
are not included in this repository.

## Limitations

- Frozen pretrained encoders were used rather than end-to-end fine-tuning.
- Some unanswerable questions remain incorrectly classified.
- Gated Fusion showed signs of overfitting.
- Further evaluation across random seeds would help assess stability.

## Ethical Considerations

Incorrect answerability predictions may affect accessibility
applications. In particular, incorrectly treating an unanswerable
question as answerable could lead to unreliable assistance.
The results should not be interpreted as evidence that the
system is ready for real-world deployment.
