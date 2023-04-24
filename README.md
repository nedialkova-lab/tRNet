# tRNet
Training and interpretation of tRNet convolutional neural network predicting tRNA gene activity from ChIP-seq peaks.

## Overview

tRNet is a convolutional neural network (CNN) architecturally based on BPNet ([GitHub](https://github.com/kundajelab/bpnet), [paper](https://doi.org/10.1038/s41588-021-00782-6). The model is aimed at specifically predicting the transcriptional activity of tRNA genes (transcribed by RNA Polymerase III) by using ChIP-seq data from the TFIIIB subunit, BRF1, to determine the activity status of predicted tRNA genes in different cellular contexts. The architecture and overview of the model and training are shown below.

<p align="left">
	<img src="/docs/img/tRNet_arch.png" width="60%" height="60%">
</p>

This final model is trained using transfer learning from a simpler binary classification model trained to predict housekeeping and inactive tRNA genes from 200bp upstream sequences (see below).

This repository contains the Jupyter notebooks to recreate the analysis in our manuscript, which includes:
* `tRNet_model_training.ipynb`: Model building and training.
* `tRNet_SHAP_tfmodisco_interpretation.ipynb`: Model interpretation using [SHAP](https://github.com/slundberg/shap) contribution scores to predict motifs with [TF-modicso](https://github.com/kundajelab/tfmodisco).

Additionally, `data/` contains fasta files with 200bp tRNA upstream sequences for housekeeping, repressed and inactive tRNAs. These classes were defined using significant BRF1 peaks called by [MACS2](https://github.com/macs3-project/MACS) overlapping tRNA genes in human induced pluripotent stem cells (hiPSC), neural progenitors (NPC), neurons and cardiomyocyte (CM) cells. Housekeeping tRNAs have peaks in all 4 cells, repressed tRNAs are inactivated during differentiation, while inactive tRNAs are never bound by BRF1 and do not produce mature tRNA in any cell type.

## Usage

Please note that the following package dependencies and versions are required to run the Jupyter notebooks. These should be installed by running the notebooks, but any errors in running especially the interpretation notebook could be due to dependency version incompatibilities.
```
tensorflow==1.15.5
keras==2.2.4
deeplift==0.6.12.0
shap==0.29.3
modisco==0.5.14.1
```

To recreate the analysis in the manuscript, please clone the GitHub repository (`clone git@github.com:drewjbeh/tRNet.git`) and run the training and interpretation notebooks. The results from this notebook are already available in this repository:
* `kFoldCV_saved_models`: Model h5 files from the K-fold cross-validation model training.
* `model`: Binary classification model and final multi-task model architecture and weights.
* `SHAP_scores`: SHAP DeepExplainer contribution scores for the multi-task model.
* `tf_modisco`: TF-modisco pattern and motif searching results using SHAP contribution scores. Included are scores for each tRNA class in hd5 and PDF outputs of actual contribution scores for each task, metacluster, pattern and activity pattern.  
