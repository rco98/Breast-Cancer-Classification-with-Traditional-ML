# 🩺 Breast Cancer Classification with Traditional ML

> A complete machine learning pipeline for breast cancer classification.

This project implements a complete traditional machine learning pipeline to classify MRI images of patients with breast cancer. It focuses on **feature extraction using PyRadiomics** and **image segmentation**.

![Python](https://img.shields.io/badge/python-3.7-blue)
![PyRadiomics](https://img.shields.io/badge/PyRadiomics-enabled-brightgreen)
![Conda](https://img.shields.io/badge/conda-environment-green)

---

## 📑 Table of Contents

1. [Project Structure](#1-project-structure)
2. [Installation and Setup](#2-installation-and-setup)
3. [Dataset](#3-dataset)
4. [Feature Extraction with PyRadiomics](#4-feature-extraction-with-pyradiomics)
5. [Preparation of the Classification Dataset and Model Training](#5-preparation-of-the-classification-dataset-and-model-training)

---

## 1. Project Structure

The recommended directory structure is as follows:

```text
BR-Classification/
├── images/                          # MRI images of patients
├── segmentations/                   # Segmentation masks
├── utilities/                       # Scripts and auxiliary files
│   ├── requirements.txt             # Python dependencies
│   ├── input_pyradiomics.csv        # CSV input file for PyRadiomics
│   ├── pyradiomics_features.csv     # CSV output file of PyRadiomics
│   └── data_info.ipynb              # Notebook for dataset exploration
└── classification/
    ├── classification.ipynb         # Notebook for training and validation
    ├── feature.csv                  # Features for the classification model
    └── label.csv                    # Labels for the classification model
```

---

## 2. Installation and Setup

### 2.1 Prerequisites

Ensure that [Anaconda](https://www.anaconda.com/) is installed on your system.

### 2.2 Create and Activate the Conda Environment

Open the Anaconda Prompt, navigate to your `BR-Classification` folder, and run:

```bash
conda create -n breast-cancer-ml-env python=3.7
conda activate breast-cancer-ml-env
```

Alternatively, to create a **local** Conda environment inside the main folder:

```bash
conda create -p ./breast-cancer-ml-env python=3.7
conda activate ./breast-cancer-ml-env
```

### 2.3 Install Dependencies

Install the required Python packages listed in `requirements.txt` (we recommend placing it inside a subfolder named `utilities` within the main project folder):

```bash
pip install -r ./utilities/requirements.txt
```

---

## 3. Dataset

### 3.1 Download the Data

Download the breast MRI images and the corresponding segmentation masks (NIfTI format) from the link below:

```text
google_drive_link
```

### 3.2 Organize the Data

Place the downloaded files inside your main project folder `BR-Classification`, in two subfolders:

- `images/`
- `segmentations/`

#### 3.2.1 Images Directory Structure

```text
images/
└── Patient_ID/
    └── Patient_ID_000X.nii.gz
```

| Element | Meaning |
| --- | --- |
| `Patient_ID/` | Unique identifier for each patient |
| `000X` | Acquisition time point of the DCE-MRI sequence |

#### 3.2.2 Segmentations Directory Structure

```text
segmentations/
├── expert/
│   └── Patient_ID.nii.gz/
└── automatic/
    └── Patient_ID.nii.gz/
```

### 3.3 Information about the Dataset

To inspect dataset metadata, use `data_info.ipynb`:

1. Download `data_info.ipynb` and place it in the `utilities` folder.
2. Launch Jupyter Lab (ensure your `breast-cancer-ml-env` is activated):

   ```bash
   jupyter lab
   ```

3. Run the notebook to explore image/mask properties such as **shape**, **data type**, **pixel spacing** and **slice thickness**.

---

## 4. Feature Extraction with PyRadiomics

PyRadiomics is an open-source Python package for the extraction of radiomics features from medical imaging. For more information, see the official [PyRadiomics documentation](https://pyradiomics.readthedocs.io/en/latest/).

### 4.1 PyRadiomics Package Installation

Install the package in your environment from the Anaconda Prompt.

Via **conda**:

```bash
conda install -c radiomics pyradiomics
```

Or via **pip**:

```bash
python -m pip install pyradiomics
```

### 4.2 Prepare the CSV File

Use the `data_info.ipynb` notebook to generate the `input_pyradiomics.csv` file required by PyRadiomics.

> [!NOTE]
> Inside the notebook, locate the section titled **"Pyradiomics Input File"**.
> It includes a code cell that creates `input_pyradiomics.csv`, which will be automatically saved in the `utilities` folder.
> Make sure to run PyRadiomics using the CSV file directly from this location to avoid any path-related issues.

### 4.3 Run PyRadiomics

Open the Anaconda Prompt and navigate to the main project folder (`BR-Classification`).

**Validate the dataset:**

```bash
pyradiomics .\utilities\input_pyradiomics.csv -o .\utilities\pyradiomics_features.csv -f csv --setting "resampledPixelSpacing: 1,1,1" --jobs 4 --validate
```

**Perform the feature extraction:**

```bash
pyradiomics .\utilities\input_pyradiomics.csv -o .\utilities\pyradiomics_features.csv -f csv --setting "resampledPixelSpacing: 1,1,1" --jobs 4
```

If successful, the extracted features will be saved in `pyradiomics_features.csv` in the `utilities` folder. This file will contain **one row per patient** and **one column for each extracted feature**.

---

## 5. Preparation of the Classification Dataset and Model Training

In this section we prepare the classification dataset, generate the required CSV files, and then train and validate classification models using the `classification.ipynb` notebook located in `.\BR-Classification\classification`.

### 5.1 Prepare the Classification Features

To generate the feature CSV file for classification, use the `classification.ipynb` notebook.

> [!NOTE]
> In the notebook, locate the section titled **"Preparation of the Classification Dataset"**. This section processes `pyradiomics_features.csv` and generates a new file named `feature.csv`.

Once executed, the resulting `feature.csv` will be saved in the `classification` folder and will be ready for use in training machine learning models.

> [!TIP]
> This file can be enriched with additional clinical information about the patients, such as:
> - Age
> - Breast cancer laterality
> - Other relevant clinical data

### 5.2 Prepare the Classification Labels

To perform classification, you need to define your clinical task (e.g. distinguishing between invasive and in situ tumors).

1. Create a `pandas` DataFrame with two columns:

   | Column | Description |
   | --- | --- |
   | `PatientID` | The unique identifier of each patient (ensure this matches the IDs used in the features file) |
   | `Label` | The target classification label for each patient (e.g. `0` = invasive, `1` = in situ) |

2. Save the DataFrame as a CSV file (e.g. `label.csv`).

This labels file will be used during model training and evaluation.

### 5.3 Train and Validate the Classification Model

Once `feature.csv` and `label.csv` are ready, use the same `classification.ipynb` notebook — starting from the paragraph **"Preparing the Feature Dataframe and Labels Array"** — to proceed with model training and validation.

#### Available Models

| Model | Model |
| --- | --- |
| XGBoost | Naive Bayes |
| K-Nearest Neighbors (KNN) | Decision Tree |
| Support Vector Machine (SVM) | Logistic Regression |
| Random Forest | Multilayer Perceptron (MLP) |

#### Validation Schemes

| Scheme | Status |
| --- | --- |
| Hold-out | 🚧 Work in progress |
| K-fold Cross-Validation | 🚧 Work in progress |
| Leave-One-Out Cross-Validation (LOOCV) | ✅ Available |

The notebook allows you to:

1. Load the `feature.csv` file with the extracted features.
2. Load the `label.csv` file with the target labels.
3. Train and validate models with the selected validation scheme.

By running the notebook, you can explore and compare different models and validation techniques with your dataset, starting the process of model training and evaluation.
