⚡ Quick Start
🧪 1. Setup Environment
conda create -n semclass python=3.9 -y
conda activate semclass

pip install torch torchvision efficientnet_pytorch pandas seaborn tqdm pillow scikit-learn matplotlib
📊 2. Generate Dataset
python aug_img.py source 500 -p 224 -t 10 -r 20 -n example -v True
🏋️ 3. Train Model
python train.py
📈 4. Evaluate
python test.py
🗂️ Project Structure
.
├── data/                # Generated datasets & outputs
├── source/              # Raw SEM images
├── aug_img.py           # Data augmentation
├── config.py            # Hyperparameters
├── datasets.py          # Dataset class
├── train.py             # Training
├── test.py              # Evaluation
├── utils.py             # Utilities
└── README.md
🔧 Key Features

✨ EfficientNet-B7 fine-tuning
✨ Config-driven experiments
✨ Deterministic training pipeline
✨ Built-in evaluation (F1, confusion matrix, CSV export)
✨ Minimal and easy-to-extend codebase

📊 Outputs
📁 Model weights (.pth)
📉 Training curves (.png)
📄 Prediction results (.csv)
📊 Confusion matrix & metrics
🧩 Customization
Task	Where
Change model	train.py, test.py
Add augmentation	utils.py
Modify hyperparameters	config.py
Change optimizer/loss	utils.py
📊 Tech Stack
<p> <img src="https://skillicons.dev/icons?i=python,pytorch" /> </p>
PyTorch
EfficientNet-B7
NumPy / Pandas
Matplotlib / Seaborn
Scikit-learn
📌 Highlights for Recruiters
✅ End-to-end ML pipeline design
✅ Strong reproducibility practices
✅ Domain expertise in materials science + AI
✅ Clean, modular, production-oriented code
📝 License

MIT License

🤝 Acknowledgements
EfficientNet (efficientnet_pytorch)
MII Research Lab for SEM datasets
