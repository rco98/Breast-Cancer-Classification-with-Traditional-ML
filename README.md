# Breast-Cancer-Classification-with-Traditional-ML

Full traditional machine learning pipeline for a breast cancer classification task.

This project requires that [Anaconda](https://www.anaconda.com/) is installed on your (Windows) system.  
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

---

## Dataset

First, download the breast MRI images and the corresponding segmentation masks in NIfTI format.  
Place them inside the main folder **`BR-Classification`**, in two subfolders named **`images`** and **`segmentation`**.

You can access and download the data from the following link:

https://drive.google.com/drive/folders/1T15ylY6xUQI6TuKlZfN_uV5z6b67eyH-?usp=sharing

### Information about the dataset

To explore the structure and metadata of the dataset, make sure to download the `data_info.ipynb` notebook and place it inside the **`utilities`** folder.

This notebook was created using [Jupyter](https://jupyter.org/), which is already included in the `breast-cancer-ml-env` environment.

To launch the notebook, activate the environment and run:
<pre lang="markdown"> 
  jupyter lab
  </pre>
This will open a new browser tab where you can navigate to and run `data_info.ipynb`.

The notebook can be modified freely according to your needs.
For now, it provides essential information about the images and masks, such as: Shape, Data type, Pixel spacing and Slice thickness.

---

## Feature extraction with pyradiomics

Pyradiomics is an open-source python package for the extraction of Radiomics features from medical imaging. For more information you can go on the official site [pyradiomics](https://pyradiomics.readthedocs.io/en/latest/).

First, we need to install the package on our environment using on Anaconda Prompt. 
As reported on the site, we can install it via conda:
<pre lang="markdown"> 
  conda install -c radiomics pyradiomics
  </pre>
or via pip:
<pre lang="markdown"> 
  python -m pip install pyradiomics
  </pre>

### Use pyradiomics on our dataset

To use Pyradiomics, it is necessary to prepare a **CSV** file that describes our dataset.  
For this purpose, the `data_info.ipynb` notebook includes a cell located in the paragraph **"Pyradiomics input file"** that generates it.

Once this cell is executed, it will create the file `input_pyradiomics.csv`, which will be saved in the **`utilities`** folder.  
**Note:** It is recommended to use `input_pyradiomics.csv` from the same folder where it was saved.

After completing this step, we can proceed to compute the features using Pyradiomics. 

Open the Anaconda Prompt. Navigate to the main project folder (**`BR-Classification`**) and first verify the integrity of the dataset by running:

<pre lang="markdown"> 
  pyradiomics .\utilities\input_pyradiomics.csv -o .\utilities\pyradiomics_features.csv -f csv --setting "resampledPixelSpacing: 1,1,1" --jobs 4 --validate
</pre>

Then, perform the actual feature extraction:

<pre lang="markdown"> 
  pyradiomics .\utilities\input_pyradiomics.csv -o .\utilities\pyradiomics_features.csv -f csv --setting "resampledPixelSpacing: 1,1,1" --jobs 4
</pre>

If the process completes successfully, you will find the file `pyradiomics_features.csv` in the **`utilities`** folder.  
This file will contain one row per patient and one column for each extracted feature.


