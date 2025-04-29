# Breast-Cancer-Classification-with-Traditional-ML

Full traditional machine learning pipeline for a breast cancer classification task.

This project requires that [Anaconda](https://www.anaconda.com/) is installed on your system.  
It is recommended to work within a dedicated folder where all project files will be organized.  
Let's assume the working directory is named **`BR-Classification`**.

---

## Dataset

First, download the breast MRI images and the corresponding segmentation masks in NIfTI format.  
Place them inside the main folder `BR-Classification`, in two subfolders named `images` and `segmentation`.

You can access and download the data from the following link:

https://drive.google.com/drive/folders/1T15ylY6xUQI6TuKlZfN_uV5z6b67eyH-?usp=sharing

## Create and activate conda environment
Once Anaconda is installed and properly configured, create a new environment that includes all the required Python packages.

Open the Anaconda Prompt, navigate to your BR-Classification folder, and run the following commands:
<pre lang="markdown"> 
  conda create -n breast-cancer-ml python=3.10
  conda activate breast-cancer-ml 
  </pre>
Alternatively, to create a local Conda environment in this folder, use:
<pre lang="markdown"> 
  conda create -p ./breast-cancer-ml python=3.10
  conda activate ./breast-cancer-ml 
  </pre>

All necessary Python packages are listed in the requirements.txt file, which can be found in this repository.
We recommend placing it inside a subfolder named utilities within the main project folder.

To install the packages, run:
<pre lang="markdown"> 
  pip install -r ./utilities/requirements.txt
  </pre>


# Install pyradiomics for feature extraction
