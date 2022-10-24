# **B**ank **A**ccount **F**raud (BAF) Tabular Dataset Suite

> **Note**
> We're pleased to announce that the paper describing this dataset suite ``Turning the Tables: Biased, Imbalanced, Dynamic Tabular Datasets for ML Evaluation'' ha been accepted to **NeurIPS 2022**. Link to paper [here](https://openreview.net/pdf?id=UrAYT2QwOX8).
> 

## Abstract

Evaluating new techniques on realistic datasets plays a crucial role in the development of ML research and its broader adoption by practitioners. In recent years, there has been a significant increase of publicly available unstructured data resources for computer vision and NLP tasks. However, tabular data — which is prevalent in many high-stakes domains — has been lagging behind. To bridge this gap, we present Bank Account Fraud (BAF), the first publicly available 1 privacy-preserving, large-scale, realistic suite of tabular datasets. The suite was generated by applying state-of-the-art tabular data generation techniques on an anonymized,real-world bank account opening fraud detection dataset. This setting carries a set of challenges that are commonplace in real-world applications, including temporal dynamics and significant class imbalance. Additionally, to allow practitioners to stress test both performance and fairness of ML methods, each dataset variant of BAF contains specific types of data bias. With this resource, we aim to provide the research community with a more realistic, complete, and robust test bed to evaluate novel and existing methods.

## Resources

> **Note**
> Dataset will be publicly available on the **12th of October 2022**,  we're still applying reviewer feedback to improve our work.

The _preprint_ paper and datasheet are available in the following links:

- [Paper](documents/BAF_paper.pdf)
- [Datasheet](documents/datasheet.pdf)

## Loading the Dataset

To load the dataset the first step is to download the variants from one of the possible resources above. The `parquet` extensions allow for smaller file sizes (approx. 3 times smaller) and quicker load speeds, at the cost of having to install an external reader package, such as `pyarrow` (if using `pandas`). Alternatively, we provide `csv` versions. If the compressed version is download, it is required to decompress it.

Then, executing the small script bellow allows to read every variant of the dataset.


```python
import glob
import pandas as pd

extension = "csv"  # or "parquet", depending on the downloaded file
data_paths = glob.glob(f"</path/to/datasets/>*.{extension}")

def read_dataset(path, ext=extension):
    if ext == "csv":
        return pd.read_csv(path, index_col=0)
    elif ext == "parquet":
        return pd.read_parquet(path)
    else:
        raise ValueError(f"Invalid extension: '{ext}'.")

def get_variant(path):
        return path.split("/")[-1].split(".")[0]

dataframes = {
    get_variant(path): read_dataset(path) for path in data_paths
}
```

This code will read every variant of the dataset available in the provided path. These are available in several `pandas.DataFrames`, discriminated by variant by the key in the `dataframes` dictionary. It is also possible to read with other engines, such as `spark`.

## Repository Structure

In this repository, you will find the code necessary to replicate the experiments of the presented paper, in the `notebooks` folder.

The [first notebook](notebooks/generate_dataset_variants.ipynb) regards the process of sampling from a large dataset to obtain the different variants that constitute the suite of datasets.

The [second notebook](notebooks/empirical_results.ipynb) presents the train of 100 LightGBM models (hyperparameters selected through random search) on the suite of datasets, as well as plots of the results. 

To replicate the environment used in the experiments, install the  `requirements.txt` file via pip in a Python 3.7 environment. 

Additionally, you can find the official documentation of the suite of datasets in the `documents` folder. In this folder you will find the [Paper](documents/BAF_paper.pdf) and [Datasheet](documents/datasheet.pdf)

The paper contains more detailed information on motivation, generation of the base dataset and [variants](notebooks/generate_dataset_variants.ipynb) and an experiment performed in the [empirical results notebook](notebooks/empirical_results.ipynb).
The datasheet contains a summarized description of the dataset. 


## Citation
```
@article{jesus2022bank,
  title={bank-account-fraud: Tabular Dataset(s) for Fraud Detection under Multiple Bias Conditions},
  author={Jesus, S{\'{e}}rgio and Pombal, Jos{\'{e}} and Alves, Duarte and Cruz, Andr{\'{e}} F and Saleiro, Pedro and Ribeiro, Rita P. and Gama, Jo{\~{a}}o and Bizarro, Pedro},
  journal={Working Paper},
  year={2022}
}
```
