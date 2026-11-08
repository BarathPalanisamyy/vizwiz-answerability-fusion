# Detecting Unanswerable Visual Questions Using Image-Text Fusion

## Project Overview

This project was completed for Multimodal Analysis Assignment 2.2 at Adelaide University.

The main aim of this project is to investigate whether combining image and text features can improve the detection of unanswerable questions in Visual Question Answering (VQA) systems.

Some images may be blurry, poorly framed, or may not contain enough information to answer a question. This is particularly important for accessibility applications, where visually impaired users may depend on these systems.

The task is treated as binary classification:

- 0: Answerable
- 1: Unanswerable

The project compares unimodal models with different image-text fusion methods to evaluate their performance and reliability.

## Dataset

The VizWiz-VQA dataset was used for this project.

Official dataset website:
https://vizwiz.org/tasks-and-datasets/vqa/

The dataset contains 32,842 image-question pairs collected from people who are blind or have low vision.

Dataset splits:

| Dataset | Samples | Unanswerable Percentage |
|---|---:|---:|
| Training | 20,523 | 27.0% |
| Validation | 4,319 | 32.1% |
| Test | 8,000 | 26.8% |

Images were resized to 224 x 224 pixels, and questions were tokenized using BERT. CLIP preprocessing was also used for the CLIP model.

## Feature Extraction

Pretrained models were used to extract features from images and questions.

1. Vision Transformer (ViT)
   - Used to extract image features.
   - Produces 768-dimensional image representations.

2. BERT
   - Used to extract question features.
   - Produces 768-dimensional text representations.

3. CLIP
   - Used to extract both image and text features.
   - Provides representations learned through image-text contrastive training.

The pretrained encoders were kept frozen during training. Only the fusion and classification layers were trained.

## Models Implemented

Seven model configurations were evaluated.

1. Question-only
   - Uses BERT question features for classification.

2. Image-only
   - Uses ViT image features for classification.

3. Early Fusion
   - Combines image and question features before classification.

4. Late Fusion
   - Combines the prediction probabilities of the unimodal models.

5. Cross-Attention Fusion
   - Uses question features to attend to image-region features.

6. Gated Fusion
   - Learns how much each modality should contribute to the prediction.

7. CLIP Early Fusion
   - Combines pretrained CLIP image and text features for classification.

## Training Configuration

The experiments were implemented using Python and PyTorch in Google Colab.

The main training settings were:

| Parameter | Value |
|---|---|
| Optimizer | AdamW |
| Learning rate | 0.001 |
| Weight decay | 0.0001 |
| Batch size | 256 |
| Maximum epochs | 15 |
| Early stopping patience | 3 |
| Random seeds | 0, 1, 2 |
| Loss function | Binary cross-entropy |

Three random seeds were used for each model to make the comparison more reliable.

Validation data was used for early stopping, model selection, and classification threshold selection. The test set was used for final evaluation.

## Evaluation Metrics

The models were mainly evaluated using:

- Macro-F1
- AUROC
- AUPRC

Additional analyses were performed to examine:

- Classification errors
- Bootstrap confidence intervals
- Training and validation performance
- Image sharpness and question frequency subgroups
- Classification threshold effects
- Inference latency

## Test Results

The table below shows the average test performance across three random seeds.

| Model | Macro-F1 | AUROC | AUPRC |
|---|---:|---:|---:|
| Question-only | 0.6568 | 0.7447 | 0.5256 |
| Image-only | 0.6946 | 0.7737 | 0.5932 |
| Early Fusion | 0.7617 | 0.8663 | 0.7235 |
| Late Fusion | 0.7409 | 0.8459 | 0.6828 |
| Cross-Attention | 0.7613 | 0.8618 | 0.7132 |
| Gated Fusion | 0.7540 | 0.8600 | 0.7113 |
| CLIP Early Fusion | 0.7990 | 0.9000 | 0.7783 |

## Key Findings

All five multimodal models performed better than the question-only and image-only baselines.

CLIP early fusion achieved the best overall performance, with a macro-F1 score of 0.7990, AUROC of 0.9000, and AUPRC of 0.7783.

Compared with the image-only baseline, CLIP early fusion improved macro-F1 by 0.1044.

Early fusion and cross-attention achieved similar results. This shows that more complex fusion methods do not always produce better performance.

The bootstrap analysis also supported the improvement achieved by CLIP early fusion.

The error and subgroup analyses showed that model performance can vary depending on image quality and question frequency.

## Repository Structure

The main project files are organized as follows:

```text
vizwiz-answerability-fusion/
|
|-- MMA_Assignment_2_2.ipynb
|-- README.md
|
|-- results/
|   |-- overfitting_comparison.png
|   |-- confusion_matrix.png
|   |-- threshold_tradeoff.png
|
|-- report/
    |-- MMA_Assignment_2_2_Report.pdf
```

The notebook contains the implementation, model training, evaluation, and visualizations.

## How to Run the Notebook

The project was developed using Google Colab, with Google Drive used to store datasets, extracted features, model files, and results.

To run the notebook:

1. Open the notebook in Google Colab.
2. Enable a GPU runtime if available.
3. Mount Google Drive.
4. Install the required Python libraries using the installation cells.
5. Run the dataset preparation and preprocessing cells.
6. Extract image and question features using the pretrained models.
7. Train the different model configurations.
8. Run the evaluation and analysis cells to generate the results.

The notebook uses Google Drive paths, so the directory locations may need to be adjusted when running it in a different environment.

The complete VizWiz dataset and large intermediate feature files are not included in this repository because of their size.

Running the full experiment may take considerable time, especially during feature extraction and model training.

## Limitations

The pretrained encoders were kept frozen to reduce computational requirements. This may limit their ability to adapt to the specific image conditions in the VizWiz dataset.

The dataset contains more answerable questions than unanswerable questions, which can affect classification performance.

The subgroup analysis also showed differences in performance across image sharpness and question frequency groups.

The latency measurements were conducted locally and did not include network delays or application overhead.

## Ethical Considerations

Incorrect answerability predictions may affect users who depend on accessibility applications.

A false positive occurs when an answerable question is incorrectly rejected. A false negative occurs when an unanswerable question is incorrectly accepted.

Both errors are important because they can reduce the reliability of the system.

The results show that multimodal fusion can improve answerability detection, but the model should not be considered ready for real-world deployment without further evaluation.

## Conclusion

This project investigated different image-text fusion approaches for detecting unanswerable visual questions.

The results showed that combining image and text features improved performance compared with using either modality alone.

CLIP early fusion achieved the best overall results among the seven models.

The project also highlighted the importance of evaluating classification errors, generalization, and reliability when developing accessibility applications.

This project was completed as part of Multimodal Analysis Assignment 2.2 at Adelaide University.
