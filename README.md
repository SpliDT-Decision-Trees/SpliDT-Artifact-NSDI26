# SpliDT Codebase

This guide describes how to install and run **SpliDT**, a partitioned decision tree training and inference framework designed for scalable, line-rate deployment.

> Note: For NSDI 2026 artifact evaluation, please follow the instructions here: [README-ARTIFACT-NSDI26.md](README-ARTIFACT-NSDI26.md).

## a. Setup

<!-- ## Prerequisites -->
<!-- !!! Dependencies -->

SpliDT is designed to be lightweight and portable. The system dependencies for the training framework are:

- **Conda** (Miniconda or Anaconda)
- **Docker**

Clone the repository as following:

```shell title="shell"
git clone https://github.com/SpliDT-Decision-Trees/SpliDT-Artifact-NSDI26.git
```
Enter the repository using the following command:
```shell title="shell"
cd SpliDT-Artifact-NSDI26
```

Then run the following command:
```shell title="shell"
git submodule update --init --recursive
```

### Conda Environment

All software dependencies are specified in `environment.yml` at the root folder of `dse-and-training-framework/`

Step 1: enter the repository
```shell title="shell"
cd dse-and-training-framework
```

Step 2: create the environment
```shell title="shell"
conda env create -f environment.yml
```

Step 3: activate the environment
```shell title="shell"
conda activate splidt
```

Verify the environment:
```shell title="shell"
python --version
```

For example, in our case, the version number is `Python 3.8.10`.

## b. Datasets
To evaluate SpliDT, we release seven real-world datasets that are used for training and evaluating partitioned decision trees.
The following are their detailed descriptions (more details in the [paper]()). 
The datasets are provided by the Canadian Institute for Cybersecurity (CIC) and UC Santa Barbara (UCSB).

| **Dataset** | **Description** | **Classes** |
|:-----------:|-----------------|:-----------:|
| CIC-IoMT2024 | A cybersecurity dataset with Internet of Medical Things (IoMT) traffic for intrusion detection in healthcare. | 19 |
| CIC-IoT2023-a | A simplified version of the CIC-IoT-2023 dataset, categorized into four primary classes of IoT traffic. | 4 |
| ISCX-VPN2016 | A dataset containing VPN and non-VPN traffic for evaluating VPN detection and privacy-related analyses. | 13 |
| CampusTraffic | UCSB campus dataset containing various application types, including web, cloud, social, and streaming traffic. | 11 |
| CIC-IoT2023-b | A comprehensive IoT dataset containing multi-class network traffic data for evaluating IoT security threats. | 32 |
| CIC-IDS2017 | A network intrusion detection dataset for various attack scenarios, including DoS, DDoS, and brute force. | 10 |
| CIC-IDS2018 | An anomaly detection dataset capturing network traffic for diverse attacks and benign activities. | 10 |

### Download 

The datasets used to train and evaluate SpliDT are publicly available via [FigShare](https://figshare.com/articles/dataset/SpliDT_Decision_Trees_Dataset_-_NSDI_2026/30944429?file=60593858).
Download and place them on your machine as following:

Step 1: download the traffic traces and pipelines
```shell title="shell"
cd ~
# download the Datasets for training and testing
curl -L -H "User-Agent: Mozilla/5.0" -OJ "https://figshare.com/ndownloader/files/60593858"
```

Step 2: unzip the downloaded files
```shell title="shell"
unzip splidt-dataset.zip
```

## c. Custom Training
Step 1: Return to the folder `SpliDT-Artifact-NSDI26/dse-and-training-framework`.
Modify the `path` field in each dataset configuration file under `configs/` to reference the absolute path of the unzipped dataset directory (e.g., `~/splidt-dataset`).
```shell title="shell"
path: "~/splidt-dataset"
```

Step 2: Modify the `script_path` field in each dataset configuration file to reference the absolute path of the hypermapper.
```shell title="shell"
script_path: "../../hypermapper/scripts/hypermapper.py"
```

Now, we are ready to train and test our datasets. 
Start by activating the conda environment you created above:
```shell title="shell"
conda activate splidt
```

Step 3: Run the following command to launch the Grafana dashboards:
```shell title="shell"
make start-dashboards
```

Step 4: Inside the `dse-and-training-framework` repository, run SpliDT training using the following command. 
You can change the configuration file argument to select the desired dataset provided under the `configs/` folder.
```shell title="shell"
python src/train.py --config configs/iscxvpn-2016-c13-bo.yml
```

> **Note:** For comparison, add the results of the baseline models and the single-partition configuration to the database, enabling visualization and plotting through Grafana (see [README-BASELINES.md](README-BASELINES.md)).
>
> ```shell title="shell"
> python src/db_pusher.py --config configs/iscxvpn-2016-c13-bo.yml
> ```

## d. Experiments and Microbenchmarks
After running training for each dataset, run the Jupyter notebook `plots/e2e-ggplot.ipynb` for the end-to-end plots and `plots/bm-ggplot.ipynb` for the microbenmark results. 

## e. Stop the Experiment

Conclude the experiments by running:

```shell title="shell"
make stop-dashboards
conda deactivate
```

## Team

* [Murayyiam Parvez](https://murayyiam-parvez.github.io) (Purdue University)
* [Annus Zulfiqar](https://annuszulfiqar2021.github.io/) (University of Michigan)
* [Sylee Beltiukov](https://maybe-hello-world.github.io/) (University of California, Santa Barbara)
* [Shir Landau Feibish](https://www.openu.ac.il/personal_sites/shir-landau-feibish/) (Open University of Israel)
* [Walter Willinger](https://acems.org.au/our-people/walter-willinger) (NIKSUN, Inc.)
* [Arpit Gupta](https://sites.cs.ucsb.edu/~arpitgupta/) (University of California, Santa Barbara)
* [Muhammad Shahbaz](https://mshahbaz.gitlab.io/) (University of Michigan)

## Contact Us

For research-related queries or collaborations:

* Email: [parvezm@purdue.edu](mailto:parvezm@purdue.edu), [zulfiqaa@umich.edu](mailto:zulfiqaa@umich.edu)
