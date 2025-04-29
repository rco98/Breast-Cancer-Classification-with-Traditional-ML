# Breast-Cancer-Classification-with-Traditional-ML

Full traditional machine learning pipeline for a breast cancer classification task.

This project requires that [Anaconda](https://www.anaconda.com/) is installed on your system.  
It is recommended to work within a dedicated folder where all project files will be organized.  
Let's assume the working directory is named **`BR-Classification`**.

---

## Create and activate conda environment
Once Anaconda is installed and properly configured, create a new environment that includes all the required Python packages.

Open the Anaconda Prompt, navigate to your BR-Classification folder, and run the following commands:
<pre lang="markdown"> 
  conda create -n breast-cancer-ml-env python=3.10
  conda activate breast-cancer-ml-env 
  </pre>
Alternatively, to create a local Conda environment in this folder, use:
<pre lang="markdown"> 
  conda create -p ./breast-cancer-ml-env python=3.10
  conda activate ./breast-cancer-ml-env 
  </pre>

All necessary Python packages are listed in the requirements.txt file, which can be found in this repository.
We recommend placing it inside a subfolder named **`utilities`** within the main project folder.

To install the packages, run:
<pre lang="markdown"> 
  pip install -r ./utilities/requirements.txt
  </pre>

**Nota** 
SCEGLIERE SE INSTALLARE PYRADIOMICS COSì O SE INSERIRE L'INSTALLAZIONE NEL PARAGRAFO DEDICATO

## Dataset

First, download the breast MRI images and the corresponding segmentation masks in NIfTI format.  
Place them inside the main folder **`BR-Classification`**, in two subfolders named **`images`** and **`segmentation`**.

You can access and download the data from the following link:

https://drive.google.com/drive/folders/1T15ylY6xUQI6TuKlZfN_uV5z6b67eyH-?usp=sharing

### Information about the dataset

To explore the structure and metadata of the dataset, this repository includes the Jupyter notebook `data_info.ipynb`, located in the **`utilities`** folder.

This notebook was created using [Jupyter](https://jupyter.org/), which is already included in the `breast-cancer-ml-env` environment.

To launch the notebook, activate the environment and run:
<pre lang="markdown"> 
  jupyter lab
  </pre>
This will open a new browser tab where you can navigate to and run `data_info.ipynb`.

The notebook can be modified freely according to your needs.
For now, it provides essential information about the images and masks, such as: Shape, Data type, Pixel spacing and Slice thickness.

