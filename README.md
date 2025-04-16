# Final Project

## Introduction

This project uses the gene expression profiles from primary and metastatic tumor samples of skin cancer patients obtained from The Cancer Genome Atlas. The goal is to classify tumor samples as either primary or metastatic using three machine learning models: Random Forest, Naive Bayes, and a 1-Dimensional Convolutional Neural Network (1D-CNN) implemented with the Conv1d Layer.

## TCGA Terminology

- **TCGA** is The Cancer Genome Atlas which is a large scale cancer genomics program that collects various types of data from multiple types of cancer.
- Each TCGA **project** corresponds to a specific cancer type or study focus. Each project contains data on cases of that cancer type, including genetic, clinical, and sometimes imaging data.
- A **case** refers to data collected from a single patient
- Each case includes multiple **samples** from that patient, such as primary tumor samples, metastatic samples, normal tissue samples, etc
- Data inside of samples can include:
  - **Gene Expression Quantification**: Indicates the expression levels of genes in the sample.
  - **Mutation Data**: Information about DNA mutations within the sample.
  - **Methylation Data**: Provides information about DNA methylation levels. Methylation, particularly in the promoter region of genes, often leads to reduced expression of that gene by hindering transcription factor binding.

## Dataset

This project only used the Gene Expression Quantification data of each sample for the classification task. The data was gathered by running the bash scripts which uses an API called gdc-client to collect the TCGA data. For simplicity and per the project requirements, the dataset was condensed to a tsv file that contains the gene expression profiles of 100 metastatic tumor samples and 100 primary tumor samples. At the bottom of this report you will find instructions to download the complete 2GB dataset.

# Running the program

The following commands should run in bash (Linux Shell). Using Git Bash, WSL, or a linux machine should suffice. Note that Python must be installed before performing any of the following steps.
&nbsp;
Note that you can clone the repo or download the zip file. Just make sure you have all files in the same directory.

## Prerequisites

Python 3.11 is required to run this program. The following package versions must be installed as well:

- NumPy version: 1.26.4
- Scikit-learn version: 1.2.2
- Pandas version: 1.5.3
- TensorFlow version: 2.18.0
  Run the following command to install the above requirements

```bash
pip install numpy==1.26.4 scikit-learn==1.2.2 pandas==1.5.3 tensorflow==2.18.0
```

## Python Executable

Execute the python file by typing the following command:

```bash
python3 finaltermproj.py
```

Note that there may be Future Warnings, please ignore them as the models train and test. You will see the resulting table at the end. The Jupyter Notebook formats the table much nicer, I recommend checking that one out!

### Jupyter File

If using the Jupyter file, you MUST run every cell in order. More specifically, make sure to run the import packages cell first. The last cell will display a nice table with all the performance metrics.

# My Results

This project evaluates the classification performance of three machine learning algorithms (Conv1D Neural Network, Naive Bayes, and Random Forest) for predicting whether tumor samples are primary or metastatic. The models were assessed using 10-fold cross-validation, and multiple performance metrics were calculated for each fold.
![alt text](image.png)
![alt text](image-1.png)
![alt text](image-2.png)

## Comparing the Algorithms

- The Conv1D model performed inconsistently across folds, with relatively low TSS and HSS values indicating difficulty in distinguishing between the two classes. While AUC is fairly strong (0.863), accuracy and balanced accuracy remain suboptimal, suggesting the need for further tuning or larger datasets for robust feature extraction. Conv1D may be unsuitable for this dataset as it performs best on sequential or time-series data where adjacent features are correlated. In gene expression profiles, the features (genes) are not inherently sequential, which may limit the effectiveness of Conv1D in capturing meaningful patterns.
- The Naive Bayes classifier showed improved performance over Conv1D, with a balanced accuracy of 74.5%. It achieved consistent precision and recall across folds, with an average F1 Score of 76.8%. However, the Brier Skill Score is slightly negative, suggesting some room for calibration or refinement.
- The Random Forest model outperformed both Conv1D and Naive Bayes in almost all metrics. With an average accuracy of 82.0% and balanced accuracy of 82.5%, it demonstrated strong classification performance. High TSS (0.650) and HSS (0.643) values indicate its effectiveness in distinguishing between primary and metastatic samples. Furthermore, a positive Brier Skill Score (0.437) and high AUC (0.892) highlight the model’s reliability and calibration.

In conclusion, Random Forest was the best-performing model, with consistently high accuracy, F1 Score, balanced accuracy, and AUC. Naive Bayes showed reasonable performance but was less robust compared to Random Forest. Conv1D Neural Network struggled to achieve competitive performance, likely due to the limited dataset size and insufficient training. These results indicate that tree-based models like Random Forest may be better suited for this dataset, while Conv1D requires additional tuning or data augmentation for improved performance.

# Downloading the Complete Dataset

To view the link of the TCGA project, [click here](https://portal.gdc.cancer.gov/projects/TCGA-SKCM).

## Downloading the data in Linux

### Step 1: Run the Data Retrieval and Counting Scripts

Start with running the data_retrieval.sh script and then the count.sh script to count the number of samples for metastatic tumor and primary tumor data.

```bash
./data_retrieval.sh
```

```bash
./count.sh
```

### Step 2: Create the manifest file

Now run the manifest_creation.sh script to create the manifest.txt file used for gdc-client download tool

```bash
./manifest_creation.sh
```

### Step 3: Download the data

Finally, run the download_data.sh script to download data inside a directory called data

```bash
./download_data.sh
```

### data/ directory Overview

Every folder in the data directory corresponds to a specific file ID from GDC (Genomic Data Commons). Each folder contains a tsv file containing gene expression data (gene expression quantification files). samples_with_expression.json is the mapping for file ID with sample metadata
