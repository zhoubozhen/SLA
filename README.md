# Source Label Adaptation
The official Pytorch implementation of "Semi-Supervised Domain Adaptation with Source Label Adaptation" accepted by CVPR 2023. Check more details of this work in our paper: [[Arxiv]](https://arxiv.org/abs/2302.02335).

## Table of Contents
- [Setting up Python Environment](#python)
- [Data Preparation](#data)
- [Setting up Wandb](#wandb)
- [Running the model](#run)
- [Citation](#citation)
- [Acknowledgement](#acknowledgement)

<a name="python"></a>
## Setting up Python Environment 

To ensure that the project runs smoothly, set up a dedicated Python environment with all the necessary packages installed. Use conda to create a new environment by running the following command:

```sh
conda create --name <env_name> python=3.10.10 --file requirements.txt
```

Replace <env_name> with the desired name of your new environment. This command will create a new environment with Python version 3.10.10 and install all the required packages specified in the requirements.txt file.

### Compatible PyTorch version

The requirements file specifies PyTorch version 2.0. Emprically it has shown to speed up the training progress.

However, the code does not use any PyTorch 2.0 features and should be compatible with older versions of PyTorch, such as version 1.12.0.

<a name="data"></a>
## Data Preparation 

### Supported Datasets

Currently, we support the following datasets:

- [DomainNet](http://ai.bu.edu/M3SDA/)
- [OfficeHome](https://www.hemanthdv.org/officeHomeDataset.html)

### Dataset Architecture

The dataset is organized into directories, as shown below:

```
- dataset_dir
    - dataset_name
        - domain 1
        - ...
        - domain N
        - text
            - domain 1
                - all.txt
                - train_1.txt
                - train_3.txt
                - test_1.txt
                - test_3.txt
                - val.txt
            - ...
            - domain N
    - ...
```

### Download and Preparation

Before running the data preparation script, make sure to update the configuration file in `data_preparation/dataset.yaml` with the correct settings for your dataset. In particular, you will need to update the `dataset_dir` variable to point to the directory where your dataset is stored.

```yaml
dataset_dir: /path/to/dataset
```

To download and prepare one of these datasets, run the following commands:

```sh
cd data_preparation
python data_preparation.py --dataset <DATASET>
```

Replace <DATASET> with the name of the dataset you want to prepare (e.g. DomainNet, OfficeHome, or Office31). This script will download the dataset (if necessary) and extract the text data which specify the way to split training, validation, and test sets. The resulting data will be saved in the format described above.

After running the data preparation script, you should be able to use the resulting data files in this repository.

<a name="wandb"></a>
## Setting up Wandb 

We use Wandb to record our experimental results. If you don't have a Wandb account, you can sign up for one [here](https://wandb.ai).

Once you have an account, you can login by running the code for the first time. The code will prompt you to login to your Wandb account.

<a name="run"></a>
## Running the model
    
### Baseline methods

To run the main Python file, use the following command:

```sh
python main.py --method mme --dataset OfficeHome --source 0 --target 1 --seed 1102 --num_iters 10000 --shot 3shot
```
    
This command runs the MME model on the 3-shot A -> C Office-Home dataset, with the specified hyperparameters. You can modify the command to run different experiments with different hyperparameters or on different datasets.

The following methods are currently supported:
1. `base`: Uses **S+T** as described in our [main paper](https://arxiv.org/abs/2302.02335).
2. `mme`: Uses **mme** as described in [this paper](https://openaccess.thecvf.com/content_ICCV_2019/papers/Saito_Semi-Supervised_Domain_Adaptation_via_Minimax_Entropy_ICCV_2019_paper.pdf).
3. `cdac`: Uses **cdac** as described in [this paper](https://openaccess.thecvf.com/content/CVPR2021/papers/Li_Cross-Domain_Adaptive_Clustering_for_Semi-Supervised_Domain_Adaptation_CVPR_2021_paper.pdf).

### Applying SLA to baseline methods

To apply our proposed SLA method, append the suffix "_SLA" to the selected method. For example:

```sh
python main.py --method mme_SLA --dataset OfficeHome --source 0 --target 1 --seed 1102 --num_iters 10000 --shot 3shot --alpha 0.3 --update_interval 500 --warmup 500 --T 0.6
```

This command runs the MME + SLA model on the 3-shot A -> C Office-Home dataset, with the specified hyperparameters. Check our main paper to find the recommended hyperparameters for each method on each dataset.

<a name="citation"></a>
## Citation

If this work is useful in your research, please cite it using the following BibTeX entry:
```bibtex
@article{yu2023semi,
  title={Semi-Supervised Domain Adaptation with Source Label Adaptation},
  author={Yu, Yu-Chu and Lin, Hsuan-Tien},
  journal={arXiv preprint arXiv:2302.02335},
  year={2023}
}
```

<a name="acknowledgement"></a>
## Acknowledgement 

This readme file is partially generated by [ChatGPT](https://chat.openai.com/chat).

This code is partially based on [MME](https://github.com/VisionLearningGroup/SSDA_MME), [CDAC](https://github.com/lijichang/CVPR2021-SSDA) and [DeepDA](https://github.com/jindongwang/transferlearning/tree/master/code/DeepDA).

The backup urls for OfficeHome, Office31 are provided at [here](https://github.com/jindongwang/transferlearning/blob/master/data/dataset.md).
