# Breast Cancer Classification with Traditional ML
A complete machine learning pipeline for breast cancer classification.

## Description
This project implements a complete traditional machine learning pipeline to classify MRI images of patients with breast cancer. It focuses on feature extraction using Pyradiomics and image segmentation.

## Table of Contents
- [1. Project Structure](#1-project-structure)
- [2. Installation and Setup](#2-installation-and-setup)
- [3. Dataset](#3-dataset)
- [4. Feature Extraction with Pyradiomics](#4-feature-extraction-with-pyradiomics)
- [5. ]


## 1. **Project Structure**

The recommended directory structure is as follows:
<pre>
    BR-Classification/
    ├── images/              # Contains MRI images of patients
    ├── segmentations/       # Contains segmentation masks
    └── utilities/           # Contains scripts and auxiliary files
        ├── requirements.txt # Python dependencies
        ├── input_pyradiomics.csv  # CSV file for Pyradiomics
        └── data_info.ipynb  # Notebook for dataset exploration
</pre>

---
## 2. **Installation and Setup**

### 2.1 Prerequisites
Ensure that [Anaconda](https://www.anaconda.com/) is installed on your system.

### 2.2 Create and Activate the Conda Environment

Open the Anaconda Prompt, navigate to your **`BR-Classification`** folder, and run the following commands:
<pre lang="markdown"> 
  conda create -n breast-cancer-ml-env python=3.10
  conda activate breast-cancer-ml-env 
  </pre>
Alternatively, to create a local Conda Environment in the main folder, use:
<pre lang="markdown"> 
  conda create -p ./breast-cancer-ml-env python=3.10
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
https://drive.google.com/drive/folders/1T15ylY6xUQI6TuKlZfN_uV5z6b67eyH-?usp=sharing
  </pre>

### 3.1 Organize the Data
Place the downloaded files inside your main project folder `BR-Classification` in two subfolders:
- `images/`  
- `segmentations/`  
#### 3.1.1 Images Directory Structure

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


### 3.2 Information about the Dataset
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
Use the `data_info.ipynb` notebook to generate the file `input_pyradiomics.csv`.

**`Note:`** 
- `data_info.ipynb` notebook includes a cell located in the paragraph **"Pyradiomics Input File"** that generates it. Once this cell is executed, it will create the file `input_pyradiomics.csv`, which will be saved in the **`utilities`** folder. It is recommended to use `input_pyradiomics.csv` from the same folder where it was saved.

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


