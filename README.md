SEM Image Classification Pipeline

A lightweight, reproducible training pipeline for classifying Scanning Electron Microscopy (SEM) images of Ni‑rich NCM cathode materials (and derivatives) using a fine‑tuned EfficientNet‑B7 backbone. The code base is intentionally minimal—no external experiment managers—so that newcomers can trace every operation from raw JPGs ➜ cropped datasets ➜ PyTorch DataLoaders ➜ training ➜ evaluation.

✨ Key Features

Component

File(s)

Purpose

Data augmentation

aug_img.py

Generates balanced train/validate/test splits by random cropping or full‑frame export of source SEM images. Handles test/val percentages and per‑class stratification.

Config registry

config.py

Central place to register dataset / version pairs and override hyper‑parameters (LR, epochs, batch size, etc.).

Dataset wrapper

datasets.py

Custom SEMDataset that assigns integer labels, supports deterministic seeding, and returns file names alongside tensors.

Training loop

train.py

Single‑GPU (or CPU) training with EfficientNet‑B7, live accuracy/loss tracking, best‑model checkpointing, and PNG learning‑curve export.

Inference & metrics

test.py

Loads the saved checkpoint, predicts on the test set, writes a CSV with per‑image results, and visualises a confusion matrix + classification report.

Utility helpers

utils.py

Normalisation statistics, DataLoader assembly, criterion/optimizer factories, and accuracy helpers.

🗂 Repository Layout

. ├── data/ # Auto‑generated datasets & artefacts │ └── example_v1/ # ↳ train/validate/test JPGs + outputs ├── source/ # Raw SEM images arranged by class label ├── aug_img.py # Data‑set builder ├── config.py # Hyper‑parameter registry ├── datasets.py # PyTorch Dataset ├── train.py # Training script ├── test.py # Evaluation script ├── utils.py # Helper functions └── README.md # (this file)

TipKeep your raw images under ./source/<class_label>/image.jpg so that the augmentation script can auto‑detect labels.

⚡ Quick Start

1. Install Dependencies

conda create -n semclass python=3.9 -y conda activate semclass pip install torch torchvision efficientnet_pytorch pandas seaborn tqdm pillow scikit-learn matplotlib

2. Prepare the Dataset

Generate 500 cropped images per class, reserving 10 % for testing and 20 % of the remainder for validation:

python aug_img.py source 500 -p 224 -t 10 -r 20 -n example -v True

Arguments

flag

default

meaning

path

source

Root folder of raw images

size

–

Number of generated images per class

-p / --pixel

True

Crop size (True = half original, full = uncropped, or explicit pixels)

-t / --test

10

% of originals reserved for the test split

-r / --ratio

20

% (of remaining) reserved for validate split

-n / --name

–

Folder name under ./data/ for the new dataset

3. Register Hyper‑parameters

Open config.py and add a new block or edit an existing one:

config = { "example": { "default": gen_default("example", size=500), "v1": {"lr": 3.5e‑4, "epoch": 40} } }

Each entry inherits from default and is merged via get_config(dataset, version) during runtime.

4. Train

python train.py

The script internally loads:

dataset_name = "example" version = "v1"

Modify those variables or expose them via environment variables if preferred.

Artifacts written to ./data/example/train/…:

example_v1.pth – best model weights (state‑dict)

example_v1.png – accuracy/loss curves

5. Evaluate

python test.py

Outputs:

Confusion‑matrix plot (on‑screen)

example_v1.csv – per‑image predictions, ground‑truth and pass/fail flag

Console classification report (precision, recall, F1)

🔧 Customisation Guide

What you want

Where to change

Use a different backbone

Replace EfficientNet.from_pretrained / .from_name in train.py & test.py.

Add data augmentation

Extend the train_img_transforms pipeline in utils.py.

Switch optimiser / loss

Add cases to utils.loss_fn & utils.optimize_fn, then reference in config.py.

Normalise inputs

Set "normalize": True in your config version; statistics are auto‑computed.

Multi‑GPU

Wrap the model in torch.nn.DataParallel before training.

📊 Reproducibility

Deterministic seeds applied to Python random, NumPy, and PyTorch.

SEMDataset accepts a fixed seed to ensure identical random crops across runs.

📝 License

This repository is released under the MIT License—see LICENSE for details.

🤝 Acknowledgements

EfficientNet‑Pytorch for the backbone implementation.

Original SEM datasets provided by the MII Research Lab.
