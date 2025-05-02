# Breast Cancer Classification with Traditional ML
A complete machine learning pipeline for breast cancer classification.

## Description
This project implements a complete traditional machine learning pipeline to classify MRI images of patients with breast cancer. It focuses on feature extraction using Pyradiomics and image segmentation.

## Table of Contents
- [1. Project Structure](#1-project-structure)
- [2. Installation and Setup](#2-installation-and-setup)
- [3. Dataset](#3-dataset)
- [4. Feature Extraction with Pyradiomics](#4-feature-extraction-with-pyradiomics)
- [5. Preparation of the Classification Dataset and Model Training](#5-preparation-of-the-classification-dataset-and-model-training)


## 1. **Project Structure**

The recommended directory structure is as follows:
<pre>
    BR-Classification/
    ├── images/              # Contains MRI images of patients
    ├── segmentations/       # Contains segmentation masks
    ├── utilities/           # Contains scripts and auxiliary files
        ├── requirements.txt # Python dependencies
        ├── input_pyradiomics.csv  # CSV input file for Pyradiomics
        ├── pyradiomics_features.csv  # CSV output file of Pyradiomics
        └── data_info.ipynb  # Notebook for dataset exploration
    └── classification
        ├── classification.ipynb  # Python dependencies
        ├── feature.csv  # CSV of features for classification model
        └── label.csv  # CSV of labels for classification model
</pre>

---
## 2. **Installation and Setup**

### 2.1 Prerequisites
Ensure that [Anaconda](https://www.anaconda.com/) is installed on your system.

### 2.2 Create and Activate the Conda Environment

Open the Anaconda Prompt, navigate to your **`BR-Classification`** folder, and run the following commands:
<pre lang="markdown"> 
  conda create -n breast-cancer-ml-env python=3.7
  conda activate breast-cancer-ml-env 
  </pre>
Alternatively, to create a local Conda Environment in the main folder, use:
<pre lang="markdown"> 
  conda create -p ./breast-cancer-ml-env python=3.7
  conda activate ./breast-cancer-ml-env 
  </pre>

### 2.3 Install Dependencies

Install the required Python packages listed in `requirements.txt` (we recommend placing it inside a subfolder named **`utilities`** within the main project folder):

<pre lang="markdown"> 
  pip install -r ./utilities/requirements.txt
  </pre>

---

## 3. **Dataset**

Explain how to download and organize the dataset.

### 3.1 Download the data 
Download the breast MRI images and the corresponding segmentation masks (NIfTI format) from the link below:  
<pre lang="markdown"> 
google_drive_link
  </pre>

### 3.2 Organize the Data
Place the downloaded files inside your main project folder `BR-Classification` in two subfolders:
- `images/`  
- `segmentations/`  
#### 3.2.1 Images Directory Structure

Your `images/` folder should follow this layout:

<pre>
images/
└── Patient_ID/
    └── Patient_ID_000X.nii.gz
</pre>

- `Patient_ID/`: unique identifier for each patient
- `000X`: acquisition time point of the DCE-MRI sequence
#### 3.1.1 Segmentations Directory Structure
Your `segmentations` directory should look like this:

<pre>
segmentations/
└── expert/
    └── Patient_ID.nii.gz/
└── automatic/
    └── Patient_ID.nii.gz/
</pre>


### 3.3 Information about the Dataset
To inspect dataset metadata, use the `data_info.ipynb`:

- Download `data_info.ipynb` and place it in the **`utilities`** folder.

- Launch Jupyter Lab (ensure your breast-cancer-ml-env is activated):
<pre lang="markdown"> 
  jupyter lab
  </pre>
- Run the notebook to explore image/mask properties such as: Shape, Data type, Pixel spacing and Slice thickness.

---

## 4. **Feature Extraction with Pyradiomics**
Pyradiomics is an open-source python package for the extraction of Radiomics features from medical imaging. For more information you can go on the official site [pyradiomics](https://pyradiomics.readthedocs.io/en/latest/).

### 4.1 Pyradomics Package Installation
Install the package on the environment using on Anaconda Prompt. 
As reported on the site, we can install it via conda:
<pre lang="markdown"> 
  conda install -c radiomics pyradiomics
  </pre>
or via pip:
<pre lang="markdown"> 
  python -m pip install pyradiomics
  </pre>

### 4.2 Prepare the CSV File

Use the `data_info.ipynb` notebook to generate the `input_pyradiomics.csv` file required by PyRadiomics.

> **Note:**  
> Inside the notebook, locate the section titled **"Pyradiomics Input File"**.  
> It includes a code cell that creates the `input_pyradiomics.csv`, which will be automatically saved in the `utilities` folder.  
> Make sure to run PyRadiomics using the CSV file directly from this location to avoid any path-related issues.

### 4.3 Run Pyradiomics
Open the Anaconda Prompt and navigate to the main project folder (**`BR-Classification`**). 
- Validate the dataset: 

<pre lang="markdown"> 
  pyradiomics .\utilities\input_pyradiomics.csv -o .\utilities\pyradiomics_features.csv -f csv --setting "resampledPixelSpacing: 1,1,1" --jobs 4 --validate
</pre>

- Perform the feature extraction:

<pre lang="markdown"> 
  pyradiomics .\utilities\input_pyradiomics.csv -o .\utilities\pyradiomics_features.csv -f csv --setting "resampledPixelSpacing: 1,1,1" --jobs 4
</pre>

If successful, the extracted features will be saved in `pyradiomics_features.csv` in the **`utilities`** folder. This file will contain one row per patient and one column for each extracted feature.

## 5. **Preparation of the Classification Dataset and Model Training**

In this section, we will prepare the classification dataset, generate the required CSV files, and then train and validate classification models using the `classification.ipynb` notebook located in the **`.\BR-Classification\classification`** folder.

### 5.1 Prepare the Classification Features
To generate the feature CSV file for classification, use the `classification.ipynb` notebook.

> **Note:**  
> In the notebook, locate the section titled **"Preparation of the Classification Dataset"**. This section processes the `pyradiomics_features.csv` file and generates a new file named `feature.csv`.

Once executed, the resulting `feature.csv` will be saved in the `classification` folder and will be ready for use in training machine learning models.

> **Note:**  
> This file can be enriched with additional clinical information about the patients, such as:
> - Age
> - Breast cancer laterality
> - Other relevant clinical data

### 5.2 Prepare the Classification Labels
To perform classification, you need to define your clinical task (e.g., distinguishing between invasive and in situ tumors).

1. Create a `pandas` DataFrame with two columns:
   - **`PatientID`**: The unique identifier of each patient (ensure this matches the IDs used in the features file).
   - **`Label`**: The target classification label for each patient (e.g., 0 = invasive, 1 = in situ).

2. Save the DataFrame as a CSV file (e.g., `label.csv`).

This labels file will be used during model training and evaluation.

### 5.3 Train and Validate the Classification Model
Once the `feature.csv` and `label.csv` files are ready, use the same `classification.ipynb` notebook, from the paragraph "**Preparing the Feature Dataframe and Labels Array**", to proceed with model training and validation.

#### Available Models
The following machine learning models are available for training:
- **XGBoost**
- **K-Nearest Neighbors (KNN)**
- **Support Vector Machine (SVM)**
- **Random Forest**
- **Naive Bayes**
- **Decision Tree**
- **Logistic Regression**
- **Multilayer Perceptron (MLP)**

#### Validation Schemes
The notebook supports the following validation schemes:
- **Hold-out** (WORK IN PROGRESS...!!!)
- **K-fold Cross-Validation** (WORK IN PROGRESS...!!!)
- **Leave-One-Out Cross-Validation (LOOCV)**

The notebook allows you to:
1. Load the `feature.csv` file with the extracted features.
2. Load the `label.csv` file with the target labels.
3. Train and validate models with the selected validation scheme.

By running the notebook, you can explore and compare different models and validation techniques with your dataset, starting the process of model training and evaluation.
