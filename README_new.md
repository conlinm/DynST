## Dynamic Survival Transformers for Causal Inference with Electronic Health Records: 
### Final Project for CS598 DL4H, Spring 2023

#### Michael Conlin, MD, MCR

This project is based upon the paper **"Dynamic Survival Transformers for Causal Inference with Electronic Health Records"**, by Prayag Chatha, Yixin Wang, Zhenke Wu, and Jeffrey Regier, which was accepted as a spotlight presentation to the [NeurIPS 2022 Workshop on Learning from Time Series for Health](https://timeseriesforhealth.github.io/).

The original paper is available here: [![arXiv](https://img.shields.io/badge/arXiv-<2210.15417>-<COLOR>.svg)](https://arxiv.org/abs/<2210.15417>)



### About the Project

The original paper describes a deep learning model for the causaul survival analysis, using both static and dynamic features. The model is built in Pytorch Lightning, dependencies are managed through Poetry, and configurations through Hydra.

This project code is adapted from the [code for the original paper.](ttps://github.com/prob-ml/DynST) I have adapted the code significantly in order to:
- run the model and experiments within Google Colab
- convert the primary python scripts into iPython notebooks
- update code to run the latest version of Pytorch and Pytorch Lightning
- incorporate weights and biases for experiment tracking

#### Data
The base data is derived from the MIMIC-III Clinical Database (https://physionet.org/content/mimiciii-demo/1.4/). We use the MIMIC-Extract pipeline to preprocess the data (https://github.com/MLforHealth/MIMIC_Extract). Further processing and creation of the synthetic survival dataset is performed using the `preprocess.py` script, or the `project_preprocess.ipynb` notebook.

#### How to Use

To begin, you will need the MIMIC-Extract file `all_hourly_data.h5` saved in a directory called `data/`. 

You can then run the original paper's code from the command line, or the modified code for this project using the iPython notebooks.

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

The model is built and run in the `project_model.ipynb` notebook. There are options to save the model weights and to run a hyperparameter sweep using Weights and Biases.

The cox proportional hazards model is built and run in the `project_coxph.ipynb` notebook, and the causal portion of the project is run in the `project_causal.ipynb` notebook.