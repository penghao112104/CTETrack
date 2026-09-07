# CTETrack: Cross-Temporal Representation Enhancement and Reliability-Aware Fusion for RGBT Tracking

Official PyTorch implementation of **CTETrack**.

The trained model and tracking results will be provided here.

### Installation

Create and activate a conda environment:

```bash
conda create -n ctetrack python=3.10
conda activate ctetrack
```

Install PyTorch and TorchVision compatible with your CUDA version. Then install the remaining dependencies:

```bash
pip install -r requirements.txt
pip install causal-conv1d mamba-ssm --no-build-isolation
```

### Data Preparation

Download the LasHeR training and testing sets. The directory structure should look like:

```text
<PATH_OF_DATASETS>/
└── LasHeR/
    ├── TrainingSet/
    │   ├── 1boygo/
    │   ├── 1handsth/
    │   └── ...
    └── TestingSet/
        ├── sequence_1/
        ├── sequence_2/
        └── ...
```

### Path Setting

Run the following command to configure the local paths:

```bash
cd <PATH_OF_CTETRACK>
python tracking/create_default_local_file.py \
  --workspace_dir . \
  --data_dir <PATH_OF_DATASETS> \
  --save_dir ./output
```

The generated path files are:

```text
lib/train/admin/local.py
lib/test/evaluation/local.py
```

### Training

Download `DropTrack_k700_800E_alldata.pth.tar` from the [official DropTrack repository](https://github.com/jimmy-dq/DropTrack) and place it under `./pretrained/`.

```bash
DATA_DIR=<PATH_OF_LASHER> \
CUDA_VISIBLE_DEVICES=0,1 \
bash train_ctetrack.sh
```

Training logs are saved under `./output/logs/`.

### Testing

#### For RGB-T benchmarks

Run the following command with the path containing only the test sequences:

```bash
SEQ_HOME=<PATH_OF_TEST_SEQUENCES> \
DATASET_NAME=LasHeR \
CUDA_VISIBLE_DEVICES=0 \
bash test_ctetrack.sh <PATH_OF_CHECKPOINT>
```

`DATASET_NAME` supports `GTOT`, `RGBT210`, `RGBT234`, and `LasHeR`. Tracking results are saved under:

```text
RGBT_workspace/results/<dataset>/CTETrack/
```

We refer you to the [LasHeR Toolkit](https://github.com/BUGPLEASEOUT/LasHeR) for LasHeR evaluation and [MPR_MSR_Evaluation](https://sites.google.com/view/ahutracking001/) for RGBT234 evaluation.

## Acknowledgment

- This repository is based on [OSTrack](https://github.com/botaoye/OSTrack).
- We use pretrained weights from [DropTrack](https://github.com/jimmy-dq/DropTrack).
- We thank the authors for releasing their code and models.
