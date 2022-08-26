# `bank-account-fraud`: Tabular Dataset(s) for Fraud Detection under Multiple Bias Conditions

## Abstract

Evaluating new ML techniques on realistic datasets plays a crucial role in the development of ML research and broader adoption by practitioners. Furthermore, with the growing ethical concerns around the potential of bias in algorithmic decision-making, fairness evaluation is becoming a standard practice in ML. However, while there has been a significant increase of publicly available unstructured datasets for computer vision and NLP tasks, there is still a lack of large-scale domain-specific tabular datasets, which hinders potential research applied to this particular domain. Additionally, many high-stakes decision-making applications, where an accurate evaluation of algorithmic fairness is of paramount importance, rely on this type of data. Ultimately, this affects the quality of the deployed models in critical applications, and consequently, automated decisions applied to people. To tackle this issue, we present `bank-account-fraud`, the first publicly available, large-scale, and privacy-preserving suite of tabular datasets for fraud detection. The suite was generated by applying state-of-the-art tabular dataset generation techniques on an anonymized, real-world bank account opening application dataset. This setting carries a set of challenges that are commonplace in real world applications, including distribution shifts in time and significant class imbalance. Additionally, to allow practitioners to `stress test` both performance and fairness of ML methods in dynamic environments, each dataset variant of the presented suite depicts a specific predetermined and controlled type of data bias, including time-related patterns. With this dataset, we hope to potentiate ML research, through a more realistic, complete and robust test bed for novel and existing ML methods. 

## Resources

The suite of datasets is available in the following links: 

- [Compressed version (csv)](https://drive.google.com/file/d/1MG3uFinxdnGeysQsKqBLrfZKlHo_LvLy/view?usp=sharing)

- [Uncompressed version (csv)](https://drive.google.com/drive/folders/1CwBQB-FAn1WKJTABA7Ww_DIW-OLEWDcP?usp=sharing)

- [Compressed version (parquet)](https://drive.google.com/file/d/1c0aArGexVMXBZGXr7XPtviAuBAmjuk03/view?usp=sharing)

- [Uncompressed version (parquet)](https://drive.google.com/drive/folders/1ZiXIfLBgm6AY5KqCrsaHyXKqUv-s9lo1?usp=sharing)

The paper and datasheet are available in the following links:

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
        return pd.read_csv(path, index=0)
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

This code will read every instance of the dataset in several `pandas.DataFrames`. It is also possible to read with other engines, such as `spark`.

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
  author={Jesus, S{\´e}rgio and Pombal, Jos{\´e} and Alves, Duarte and Cruz, Andr{\´e} F. and Saleiro, Pedro and Ribeiro, Rita P. and Gama, Jo{\~a}o and Bizarro, Pedro},
  journal={Working Paper},
  year={2022}
}
```
