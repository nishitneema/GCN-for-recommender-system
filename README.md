# Recommenders

[![Documentation Status](https://readthedocs.org/projects/microsoft-recommenders/badge/?version=latest)](https://microsoft-recommenders.readthedocs.io/en/latest/?badge=latest)

## What's New (April, 2023)

We reached 15,000 stars!!

Our latest release is [Recommenders 1.1.1](https://github.com/microsoft/recommenders/releases/tag/1.1.1)! 

We have introduced a new way of testing our repository using [AzureML](https://azure.microsoft.com/en-us/services/machine-learning/). With AzureML we are able to distribute our tests to different machines and run them in parallel. This allows us to test our repository on a wider range of machines and provides us with a much faster test cycle. Our total computation time went from around 9h to 35min, and we were able to reduce the costs by half. See more details [here](tests/README.md).

We also made other improvements like faster evaluation metrics and improving SAR algorithm. 

Starting with release 0.6.0, Recommenders has been available on PyPI and can be installed using pip! 

Here you can find the PyPi page: https://pypi.org/project/recommenders/

Here you can find the package documentation: https://microsoft-recommenders.readthedocs.io/en/latest/

## Introduction

This repository contains examples and best practices for building recommendation systems, provided as Jupyter notebooks. The examples detail our learnings on five key tasks:

- [Prepare Data](examples/01_prepare_data): Preparing and loading data for each recommender algorithm
- [Model](examples/00_quick_start): Building models using various classical and deep learning recommender algorithms such as Alternating Least Squares ([ALS](https://spark.apache.org/docs/latest/api/python/_modules/pyspark/ml/recommendation.html#ALS)) or eXtreme Deep Factorization Machines ([xDeepFM](https://arxiv.org/abs/1803.05170)).
- [Evaluate](examples/03_evaluate): Evaluating algorithms with offline metrics
- [Model Select and Optimize](examples/04_model_select_and_optimize): Tuning and optimizing hyperparameters for recommender models
- [Operationalize](examples/05_operationalize): Operationalizing models in a production environment on Azure

Several utilities are provided in [recommenders](recommenders) to support common tasks such as loading datasets in the format expected by different algorithms, evaluating model outputs, and splitting training/test data. Implementations of several state-of-the-art algorithms are included for self-study and customization in your own applications. See the [Recommenders documentation](https://readthedocs.org/projects/microsoft-recommenders/).

For a more detailed overview of the repository, please see the documents on the [wiki page](https://github.com/microsoft/recommenders/wiki/Documents-and-Presentations).

## Getting Started

We recommend [conda](https://docs.conda.io/projects/conda/en/latest/glossary.html?highlight=environment#conda-environment) for environment management, and [vscode](https://code.visualstudio.com/) for development. To install the recommenders package and run an example notebook:

```bash
# Create and activate a new conda environment
conda create -n <environment_name> python=3.9
conda activate <environment_name>

# Install the recommenders package with examples
pip install recommenders[examples]

# create a Jupyter kernel
python -m ipykernel install --user --name <environment_name> --display-name <kernel_name>

# Clone this repo within vscode or using command:
git clone https://github.com/microsoft/recommenders.git

# Within vscode:
#   1. Open a notebook, e.g., examples/00_quick_start/sar_movielens.ipynb;  
#   2. Select Jupyter kernel <kernel_name>;
#   3. Run the notebook.
```

For more information about setup including extras, as well as configurations for GPU, Spark and Docker container, see the [setup guide](SETUP.md).

In addition to the core package, several extras are also provided, including:
+ `[examples]`: Needed for running examples.
+ `[gpu]`: Needed for running GPU models.
+ `[spark]`: Needed for running Spark models.
+ `[dev]`: Needed for development for the repo.
+ `[all]`: `[examples]`|`[gpu]`|`[spark]`|`[dev]`
+ `[experimental]`: Models that are not throughly tested and/or may require additional steps in installation.
+ `[nni]`: Needed for running models integrated with [NNI](https://nni.readthedocs.io/en/stable/).


## Algorithms

The table below lists the recommender algorithms currently available in the repository. Notebooks are linked under the Example column as Quick start, showcasing an easy to run example of the algorithm, or as Deep dive, explaining in detail the math and implementation of the algorithm.

| Algorithm | Type | Description | Example |
|-----------|------|-------------|---------|
| Alternating Least Squares (ALS) | Collaborative Filtering | Matrix factorization algorithm for explicit or implicit feedback in large datasets, optimized for scalability and distributed computing capability. It works in the PySpark environment. | [Quick start](examples/00_quick_start/als_movielens.ipynb) / [Deep dive](examples/02_model_collaborative_filtering/als_deep_dive.ipynb) |
| Attentive Asynchronous Singular Value Decomposition (A2SVD)<sup>*</sup> | Collaborative Filtering | Sequential-based algorithm that aims to capture both long and short-term user preferences using attention mechanism. It works in the CPU/GPU environment. | [Quick start](examples/00_quick_start/sequential_recsys_amazondataset.ipynb) |
| Cornac/Bayesian Personalized Ranking (BPR) | Collaborative Filtering | Matrix factorization algorithm for predicting item ranking with implicit feedback. It works in the CPU environment. | [Deep dive](examples/02_model_collaborative_filtering/cornac_bpr_deep_dive.ipynb) |
| Cornac/Bilateral Variational Autoencoder (BiVAE) | Collaborative Filtering | Generative model for dyadic data (e.g., user-item interactions). It works in the CPU/GPU environment. | [Deep dive](examples/02_model_collaborative_filtering/cornac_bivae_deep_dive.ipynb) |
| Convolutional Sequence Embedding Recommendation (Caser) | Collaborative Filtering | Algorithm based on convolutions that aim to capture both user’s general preferences and sequential patterns. It works in the CPU/GPU environment. | [Quick start](examples/00_quick_start/sequential_recsys_amazondataset.ipynb) |
| Deep Knowledge-Aware Network (DKN)<sup>*</sup> | Content-Based Filtering | Deep learning algorithm incorporating a knowledge graph and article embeddings for providing news or article recommendations. It works in the CPU/GPU environment. | [Quick start](examples/00_quick_start/dkn_MIND.ipynb) / [Deep dive](examples/02_model_content_based_filtering/dkn_deep_dive.ipynb) |
| Extreme Deep Factorization Machine (xDeepFM)<sup>*</sup> | Hybrid | Deep learning based algorithm for implicit and explicit feedback with user/item features. It works in the CPU/GPU environment. | [Quick start](examples/00_quick_start/xdeepfm_criteo.ipynb) |
| FastAI Embedding Dot Bias (FAST) | Collaborative Filtering | General purpose algorithm with embeddings and biases for users and items. It works in the CPU/GPU environment. | [Quick start](examples/00_quick_start/fastai_movielens.ipynb) |
| LightFM/Hybrid Matrix Factorization | Hybrid | Hybrid matrix factorization algorithm for both implicit and explicit feedbacks. It works in the CPU environment. | [Quick start](examples/02_model_hybrid/lightfm_deep_dive.ipynb) |
| LightGBM/Gradient Boosting Tree<sup>*</sup> | Content-Based Filtering | Gradient Boosting Tree algorithm for fast training and low memory usage in content-based problems. It works in the CPU/GPU/PySpark environments. | [Quick start in CPU](examples/00_quick_start/lightgbm_tinycriteo.ipynb) / [Deep dive in PySpark](examples/02_model_content_based_filtering/mmlspark_lightgbm_criteo.ipynb) |
| LightGCN | Collaborative Filtering | Deep learning algorithm which simplifies the design of GCN for predicting implicit feedback. It works in the CPU/GPU environment. | [Deep dive](examples/02_model_collaborative_filtering/lightgcn_deep_dive.ipynb) |
| GeoIMC<sup>*</sup> | Hybrid | Matrix completion algorithm that has into account user and item features using Riemannian conjugate gradients optimization and following a geometric approach. It works in the CPU environment. | [Quick start](examples/00_quick_start/geoimc_movielens.ipynb) |
| GRU4Rec | Collaborative Filtering | Sequential-based algorithm that aims to capture both long and short-term user preferences using recurrent neural networks. It works in the CPU/GPU environment. | [Quick start](examples/00_quick_start/sequential_recsys_amazondataset.ipynb) |
| Multinomial VAE | Collaborative Filtering | Generative model for predicting user/item interactions. It works in the CPU/GPU environment. | [Deep dive](examples/02_model_collaborative_filtering/multi_vae_deep_dive.ipynb) |
| Neural Recommendation with Long- and Short-term User Representations (LSTUR)<sup>*</sup> | Content-Based Filtering | Neural recommendation algorithm for recommending news articles with long- and short-term user interest modeling. It works in the CPU/GPU environment. | [Quick start](examples/00_quick_start/lstur_MIND.ipynb) |
| Neural Recommendation with Attentive Multi-View Learning (NAML)<sup>*</sup> | Content-Based Filtering | Neural recommendation algorithm for recommending news articles with attentive multi-view learning. It works in the CPU/GPU environment. | [Quick start](examples/00_quick_start/naml_MIND.ipynb) |
| Neural Collaborative Filtering (NCF) | Collaborative Filtering | Deep learning algorithm with enhanced performance for user/item implicit feedback. It works in the CPU/GPU environment.| [Quick start](examples/00_quick_start/ncf_movielens.ipynb) / [Deep dive](examples/02_model_collaborative_filtering/ncf_deep_dive.ipynb) |
| Neural Recommendation with Personalized Attention (NPA)<sup>*</sup> | Content-Based Filtering | Neural recommendation algorithm for recommending news articles with personalized attention network. It works in the CPU/GPU environment. | [Quick start](examples/00_quick_start/npa_MIND.ipynb) |
| Neural Recommendation with Multi-Head Self-Attention (NRMS)<sup>*</sup> | Content-Based Filtering | Neural recommendation algorithm for recommending news articles with multi-head self-attention. It works in the CPU/GPU environment. | [Quick start](examples/00_quick_start/nrms_MIND.ipynb) |
| Next Item Recommendation (NextItNet) | Collaborative Filtering | Algorithm based on dilated convolutions and residual network that aims to capture sequential patterns. It considers both user/item interactions and features.  It works in the CPU/GPU environment. | [Quick start](examples/00_quick_start/sequential_recsys_amazondataset.ipynb) |
| Restricted Boltzmann Machines (RBM) | Collaborative Filtering | Neural network based algorithm for learning the underlying probability distribution for explicit or implicit user/item feedback. It works in the CPU/GPU environment. | [Quick start](examples/00_quick_start/rbm_movielens.ipynb) / [Deep dive](examples/02_model_collaborative_filtering/rbm_deep_dive.ipynb) |
| Riemannian Low-rank Matrix Completion (RLRMC)<sup>*</sup> | Collaborative Filtering | Matrix factorization algorithm using Riemannian conjugate gradients optimization with small memory consumption to predict user/item interactions. It works in the CPU environment. | [Quick start](examples/00_quick_start/rlrmc_movielens.ipynb) |
| Simple Algorithm for Recommendation (SAR)<sup>*</sup> | Collaborative Filtering | Similarity-based algorithm for implicit user/item feedback.  It works in the CPU environment. | [Quick start](examples/00_quick_start/sar_movielens.ipynb) / [Deep dive](examples/02_model_collaborative_filtering/sar_deep_dive.ipynb) |
| Self-Attentive Sequential Recommendation (SASRec) | Collaborative Filtering | Transformer based algorithm for sequential recommendation. It works in the CPU/GPU environment. | [Quick start](examples/00_quick_start/sasrec_amazon.ipynb) |
| Short-term and Long-term Preference Integrated Recommender (SLi-Rec)<sup>*</sup> | Collaborative Filtering | Sequential-based algorithm that aims to capture both long and short-term user preferences using attention mechanism, a time-aware controller and a content-aware controller. It works in the CPU/GPU environment. | [Quick start](examples/00_quick_start/sequential_recsys_amazondataset.ipynb) |
| Multi-Interest-Aware Sequential User Modeling (SUM)<sup>*</sup> | Collaborative Filtering | An enhanced memory network-based sequential user model which aims to capture users' multiple interests. It works in the CPU/GPU environment. | [Quick start](examples/00_quick_start/sequential_recsys_amazondataset.ipynb) |
| Sequential Recommendation Via Personalized Transformer (SSEPT) | Collaborative Filtering | Transformer based algorithm for sequential recommendation with User embedding. It works in the CPU/GPU environment. | [Quick start](examples/00_quick_start/sasrec_amazon.ipynb) |
| Standard VAE | Collaborative Filtering | Generative Model for predicting user/item interactions.  It works in the CPU/GPU environment. | [Deep dive](examples/02_model_collaborative_filtering/standard_vae_deep_dive.ipynb) |
| Surprise/Singular Value Decomposition (SVD) | Collaborative Filtering | Matrix factorization algorithm for predicting explicit rating feedback in small datasets. It works in the CPU/GPU environment. | [Deep dive](examples/02_model_collaborative_filtering/surprise_svd_deep_dive.ipynb) |
| Term Frequency - Inverse Document Frequency (TF-IDF) | Content-Based Filtering | Simple similarity-based algorithm for content-based recommendations with text datasets. It works in the CPU environment. | [Quick  start](examples/00_quick_start/tfidf_covid.ipynb) |
| Vowpal Wabbit (VW)<sup>*</sup> | Content-Based Filtering | Fast online learning algorithms, great for scenarios where user features / context are constantly changing. It uses the CPU for online learning. | [Deep dive](examples/02_model_content_based_filtering/vowpal_wabbit_deep_dive.ipynb) |
| Wide and Deep | Hybrid | Deep learning algorithm that can memorize feature interactions and generalize user features. It works in the CPU/GPU environment. | [Quick start](examples/00_quick_start/wide_deep_movielens.ipynb) |
| xLearn/Factorization Machine (FM) & Field-Aware FM (FFM) | Hybrid | Quick and memory efficient algorithm to predict labels with user/item features. It works in the CPU/GPU environment. | [Deep dive](examples/02_model_hybrid/fm_deep_dive.ipynb) |

**NOTE**: <sup>*</sup> indicates algorithms invented/contributed by Microsoft.

Independent or incubating algorithms and utilities are candidates for the [contrib](contrib) folder. This will house contributions which may not easily fit into the core repository or need time to refactor or mature the code and add necessary tests.

| Algorithm | Type | Description | Example |
|-----------|------|-------------|---------|
| SARplus <sup>*</sup> | Collaborative Filtering | Optimized implementation of SAR for Spark |  [Quick start](contrib/sarplus/README.md) |

### Algorithm Comparison

We provide a [benchmark notebook](examples/06_benchmarks/movielens.ipynb) to illustrate how different algorithms could be evaluated and compared. In this notebook, the MovieLens dataset is split into training/test sets at a 75/25 ratio using a stratified split. A recommendation model is trained using each of the collaborative filtering algorithms below. We utilize empirical parameter values reported in literature [here](http://mymedialite.net/examples/datasets.html). For ranking metrics we use `k=10` (top 10 recommended items). We run the comparison on a Standard NC6s_v2 [Azure DSVM](https://azure.microsoft.com/en-us/services/virtual-machines/data-science-virtual-machines/) (6 vCPUs, 112 GB memory and 1 P100 GPU). Spark ALS is run in local standalone mode. In this table we show the results on Movielens 100k, running the algorithms for 15 epochs.

| Algo | MAP | nDCG@k | Precision@k | Recall@k | RMSE | MAE | R<sup>2</sup> | Explained Variance |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ALS](examples/00_quick_start/als_movielens.ipynb) | 0.004732 |	0.044239 |	0.048462 |	0.017796 | 0.965038 |	0.753001 |	0.255647 |	0.251648 |
| [BiVAE](examples/02_model_collaborative_filtering/cornac_bivae_deep_dive.ipynb) | 0.146126	| 0.475077 |	0.411771 |	0.219145 | N/A |	N/A |	N/A |	N/A |
| [BPR](examples/02_model_collaborative_filtering/cornac_bpr_deep_dive.ipynb) | 0.132478	| 0.441997 |	0.388229 |	0.212522 | N/A |	N/A |	N/A |	N/A |
| [FastAI](examples/00_quick_start/fastai_movielens.ipynb) | 0.025503 |	0.147866 |	0.130329 |	0.053824 | 0.943084 |	0.744337 |	0.285308 |	0.287671 |
| [LightGCN](examples/02_model_collaborative_filtering/lightgcn_deep_dive.ipynb) | 0.088526 | 0.419846 | 0.379626 | 0.144336 | N/A | N/A | N/A | N/A |
| [NCF](examples/02_model_hybrid/ncf_deep_dive.ipynb) | 0.107720	| 0.396118 |	0.347296 |	0.180775 | N/A | N/A | N/A | N/A |
| [SAR](examples/00_quick_start/sar_movielens.ipynb) | 0.110591 |	0.382461 | 	0.330753 | 0.176385 | 1.253805 | 1.048484 |	-0.569363 |	0.030474 |
| [SVD](examples/02_model_collaborative_filtering/surprise_svd_deep_dive.ipynb) | 0.012873	| 0.095930 |	0.091198 |	0.032783 | 0.938681 | 0.742690 | 0.291967 | 0.291971 |

## Code of Conduct

This project adheres to [Microsoft's Open Source Code of Conduct](CODE_OF_CONDUCT.md) in order to foster a welcoming and inspiring community for all.

## Contributing

This project welcomes contributions and suggestions. Before contributing, please see our [contribution guidelines](CONTRIBUTING.md).

## Build Status

These tests are the nightly builds, which compute the smoke and integration tests. `main` is our principal branch and `staging` is our development branch. We use [pytest](https://docs.pytest.org/) for testing python utilities in [recommenders](recommenders) and [Papermill](https://github.com/nteract/papermill) and [Scrapbook](https://nteract-scrapbook.readthedocs.io/en/latest/) for the [notebooks](examples). 

For more information about the testing pipelines, please see the [test documentation](tests/README.md).

### AzureML Nightly Build Status

Smoke and integration tests are run daily on AzureML.

| Build Type | Branch | Status |  | Branch | Status |
| --- | --- | --- | --- | --- | --- |
| **Linux CPU** | main | [![azureml-cpu-nightly](https://github.com/microsoft/recommenders/actions/workflows/azureml-cpu-nightly.yml/badge.svg?branch=main)](https://github.com/microsoft/recommenders/actions/workflows/azureml-cpu-nightly.yml?query=branch%3Amain) | | staging | [![azureml-cpu-nightly](https://github.com/microsoft/recommenders/actions/workflows/azureml-cpu-nightly.yml/badge.svg?branch=staging)](https://github.com/microsoft/recommenders/actions/workflows/azureml-cpu-nightly.yml?query=branch%3Astaging) |
| **Linux GPU** | main | [![azureml-gpu-nightly](https://github.com/microsoft/recommenders/actions/workflows/azureml-gpu-nightly.yml/badge.svg?branch=main)](https://github.com/microsoft/recommenders/actions/workflows/azureml-gpu-nightly.yml?query=branch%3Amain) | | staging | [![azureml-gpu-nightly](https://github.com/microsoft/recommenders/actions/workflows/azureml-gpu-nightly.yml/badge.svg?branch=staging)](https://github.com/microsoft/recommenders/actions/workflows/azureml-gpu-nightly.yml?query=branch%3Astaging) |
| **Linux Spark** | main | [![azureml-spark-nightly](https://github.com/microsoft/recommenders/actions/workflows/azureml-spark-nightly.yml/badge.svg?branch=main)](https://github.com/microsoft/recommenders/actions/workflows/azureml-spark-nightly.yml?query=branch%3Amain) | | staging | [![azureml-spark-nightly](https://github.com/microsoft/recommenders/actions/workflows/azureml-spark-nightly.yml/badge.svg?branch=staging)](https://github.com/microsoft/recommenders/actions/workflows/azureml-spark-nightly.yml?query=branch%3Astaging) |

## References

- D. Li, J. Lian, L. Zhang, K. Ren, D. Lu, T. Wu, X. Xie, "Recommender Systems: Frontiers and Practices" (in Chinese), Publishing House of Electronics Industry, Beijing 2022.
- A. Argyriou, M. González-Fierro, and L. Zhang, "Microsoft Recommenders: Best Practices for Production-Ready Recommendation Systems", *WWW 2020: International World Wide Web Conference Taipei*, 2020. Available online: https://dl.acm.org/doi/abs/10.1145/3366424.3382692
- L. Zhang, T. Wu, X. Xie, A. Argyriou, M. González-Fierro and J. Lian, "Building Production-Ready Recommendation System at Scale", *ACM SIGKDD Conference on Knowledge Discovery and Data Mining 2019 (KDD 2019)*, 2019.
- S. Graham,  J.K. Min, T. Wu, "Microsoft recommenders: tools to accelerate developing recommender systems", *RecSys '19: Proceedings of the 13th ACM Conference on Recommender Systems*, 2019. Available online: https://dl.acm.org/doi/10.1145/3298689.3346967
