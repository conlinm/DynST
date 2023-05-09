## Dynamic Survival Transformers for Causal Inference with Electronic Health Records: 
### Final Project for CS598 DL4H, Spring 2023

#### Michael Conlin, MD, MCR

This project is based upon the paper **"Dynamic Survival Transformers for Causal Inference with Electronic Health Records"**, by Prayag Chatha, Yixin Wang, Zhenke Wu, and Jeffrey Regier, which was accepted as a spotlight presentation to the [NeurIPS 2022 Workshop on Learning from Time Series for Health](https://timeseriesforhealth.github.io/).

The original paper is available here: [![arXiv](https://img.shields.io/badge/arXiv-<2210.15417>-<COLOR>.svg)](https://arxiv.org/abs/<2210.15417>)



### About the Project

The original paper describes a deep learning model for the causaul survival analysis, using both static and dynamic features. The model is built in Pytorch Lightning, dependencies are managed through Poetry, and configurations through Hydra.

This project code is adapted from the [code for the original paper.](https://github.com/prob-ml/DynST) I have adapted the code significantly in order to:
- run the model and experiments within Google Colab
- convert the primary python scripts into iPython notebooks
- update code to run the latest version of Pytorch and Pytorch Lightning
- incorporate weights and biases for experiment tracking

#### Data
The data is derived from the MIMIC-III Clinical Database (https://physionet.org/content/mimiciii-demo/1.4/). We use the MIMIC-Extract pipeline to preprocess the data (https://github.com/MLforHealth/MIMIC_Extract). Further processing and creation of the synthetic survival dataset is performed using the `preprocess.py` script, or the `project_preprocess.ipynb` notebook.

#### How to Use

To begin, you will need the MIMIC-Extract file `all_hourly_data.h5` saved in a directory called `data/`. This file is available directly from [this google cloud folder](https://console.cloud.google.com/storage/browser/mimic_extract;tab=objects?prefix=&forceOnObjectsSortingFiltering=false) with the appropriate priveledges from physionet.org. More information can be found at the link above to the MIMIC-Extract repository.

You can then run the original paper's code from the command line, or the modified code for this project using the iPython notebooks.

##### Primary Dependencies
The primary dependencies for this project are:
- Pytorch
- Pytorch Lightning
- Hydra (used with original code)
- Weights and Biases (optional)
- Pandas
- Numpy
- Scikit-learn
- Matplotlib
- Lifelines
  
The original code dependencies are managed through Poetry, and the project code in the iPython notebooks are designed to run with the latest versions of Pytorch and Pytorch Lightning, on Google Colab. The notebooks will install the necessary dependencies.

##### Running the Original Code

To run the original code, you will need to install Poetry, and then install the dependencies from the `pyproject.toml` file.

To generate the semi-synthetic dataset, you should then run
```
python run.py preprocess.do=True
```
To launch a multirun of DynST sweeping over several hyperparameters, you can enter the following command:

```
python run -m model.d_model=32,64 model.alpha=0,0.1,0.2  
```

##### Running the Project Code

First prepare the data by running the `project_preprocess.ipynb` notebook. This will generate the semi-synthetic dataset and save it to the `data/` directory.

The DynST model is built and run in the `project_model.ipynb` notebook. There are options to save the model weights and to run a hyperparameter sweep using Weights and Biases.

The cox proportional hazards model is built and run in the `project_coxph.ipynb` notebook, and the causal portion of the project is run in the `project_causal.ipynb` notebook. I have not been able yet to run the causal model in Google Colab due to environment issues, but it should run in a local environment with the appropriate dependencies installed.

#### Results

I was able to reproduce the results of the original paper and confirm the conclusions.

##### Predictive model results
(reported in Mean Absolute Error)

| Model       | MAE (reproduction) | MAE (reported)    | \% difference |
|-------------|-------------------|------------------|---------------|
| Cox Model | 16.04 +/- 0.25  | 16.04 +/- 0.25 | 0             |
| Static ST | 11.42 +/- 0.23  | 11.42 +/- 0.23 | 0             |
| DynST     | 11.17 +/- 0.72  | 11.19 +/- 0.24 | 0.2         |


##### Causal Inference Results
(the estimators of average treatment effect on RMST is reported as the bias +/- standard deviation)

| Model          | 	tau = 8          | tau = 12         | tau = 16         |
|----------------|--------------------|---------------------|---------------------|
| Cox Model    | -0.260 +/- 0.001 | -0.523 +/- 0.0012 | -0.750 +/- 0.0080 |
| Logistic IPW | 0.257 +/- 0.0    | 0.35 +/- 0.0      | 0.416 +/- 0.0     |
| DynST        | -0.146 +/- 0.041 | -0.160 +/- 0.078  | -0.190 +/- 0.12   |
