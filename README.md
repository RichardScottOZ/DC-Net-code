# DC-Net: Deep Learning for Gravity Field Downward Continuation

DC-Net is a deep learning framework for downward continuation of gravity field data. This project implements an encoder-decoder architecture with adversarial training to accurately predict gravity anomalies at different altitudes.

## Overview

Downward continuation is a crucial technique in geophysics for transforming potential field data (such as gravity measurements) from one observation level to another at a lower altitude. This process is mathematically unstable and amplifies noise, making it challenging with traditional methods. DC-Net addresses these challenges using deep learning.

### Key Features

- **Deep Learning Architecture**: Encoder-decoder network with adversarial training
- **Noise Robustness**: Trained to handle noisy gravity data
- **Multiple Continuation Heights**: Supports various continuation distances (e.g., 500m, 1000m)
- **Fast Processing**: GPU-accelerated inference for rapid predictions
- **Comprehensive Notebooks**: Multiple Jupyter notebooks for training, testing, and real data processing

## Architecture

The DC-Net framework consists of three main components:

1. **GravEncoder** (`encoder.py`): Encodes gravity field observations into a latent representation
2. **GravDecoder** (`decoder.py`): Decodes the representation into 3D density models and predicts gravity at target altitude
3. **GravDiscriminator** (`discriminator.py`): Adversarial component that ensures realistic density distributions

## Installation

### Requirements

- Python 3.7+
- PyTorch 1.8+
- NumPy
- Matplotlib
- [GeoIST](https://github.com/gravity-igpcea/geoist) - Geophysical Inversion and Simulation Tools
- Jupyter Notebook (for running the notebooks)

### Setup

```bash
# Clone the repository
git clone https://github.com/RichardScottOZ/DC-Net-code.git
cd DC-Net-code

# Install dependencies
pip install torch numpy matplotlib pandas scipy
pip install geoist
pip install jupyter

# For GPU support (recommended)
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```

## Quick Start

### Training

Train the DC-Net model with synthetic data:

```bash
python train.py --checkpoint ./models/checkpoint.pt --device cuda:0 --epochs 600 --batch_size 64
```

Key training parameters:
- `--checkpoint`: Path to save/load model checkpoint
- `--device`: Computing device (cuda:0 or cpu)
- `--epochs`: Number of training epochs
- `--batch_size`: Training batch size
- `--gp`: Gradient penalty weight (default: 10)

### Inference

Use a trained model for downward continuation:

```python
import torch
import numpy as np
import encoder
import decoder

# Load trained model
checkpoint = torch.load('./models/checkpointSH_noisefree_500m_adj.pt')
netEnc = encoder.GravEncoder()
netEnc.load_state_dict(checkpoint['enc_state_dict'])
netEnc.eval()

# Process gravity data
with torch.no_grad():
    input_data = torch.from_numpy(gravity_data).float()
    input_data = input_data.reshape(1, 1, 64, 64)
    output = netEnc(input_data)
```

## Jupyter Notebooks

The repository includes both Chinese and English versions of demonstration notebooks:

### English Notebooks

1. **traditional_continuation_method.ipynb** (传统延拓方法.ipynb)
   - Demonstrates traditional downward continuation methods
   - Compares DC-Net with classical approaches
   - Shows the advantages of deep learning

2. **dataset_acquisition.ipynb** (获取数据集.ipynb)
   - Generates synthetic training datasets
   - Creates gravity forward modeling data
   - Prepares data with various noise levels

3. **method_comparison.ipynb** (方法对比.ipynb)
   - Comprehensive comparison of different continuation methods
   - Evaluates DC-Net performance against traditional techniques
   - Quantitative metrics and visualizations

4. **results_testing.ipynb** (结果测试.ipynb)
   - Tests trained models on synthetic data
   - Validates model performance
   - Loads and evaluates checkpoints

5. **results_prediction.ipynb** (结果预测.ipynb)
   - Applies trained models to make predictions
   - Exports models to ONNX format
   - Demonstrates inference workflow

6. **actual_data_processing.ipynb** (实际资料处理.ipynb)
   - Processes real gravity survey data
   - Applies DC-Net to field measurements
   - Complete workflow from raw data to results

### Running Notebooks

```bash
# Start Jupyter Notebook
jupyter notebook

# Navigate to any notebook and run cells sequentially
```

## Project Structure

```
DC-Net-code/
├── README.md                          # This file
├── train.py                           # Main training script
├── encoder.py                         # Encoder network architecture
├── decoder.py                         # Decoder network architecture
├── discriminator.py                   # Discriminator for adversarial training
├── data_get.py                        # Dataset generation utilities
├── datagen.py                         # Data generation helpers
├── check_data.py                      # Data validation utilities
├── ops.py                             # Network operations
├── models/                            # Trained model checkpoints (user created)
│   ├── checkpointSH_noisefree_500m_adj.pt
│   ├── checkpointSH_withnoise_500m.pt
│   └── ...
└── *.ipynb                            # Jupyter notebooks (Chinese and English)
```

## Model Checkpoints

The project supports different model checkpoints for various scenarios:

- `checkpointSH_noisefree_500m_adj.pt`: Trained on noise-free data, 500m continuation
- `checkpointSH_withnoise_500m.pt`: Trained with noise, 500m continuation
- `checkpointSH_withnoise_500m_adj2.pt`: Adjusted version with noise handling

Create a `models/` directory to store your checkpoints:

```bash
mkdir -p models
```

## Data Format

Input gravity data should be:
- Shape: (64, 64) grid
- Units: mGal (milligals) or SI units
- Format: NumPy array or PyTorch tensor

The decoder expects density models in the format:
- Shape: (nz, ny, nx) - typically (32, 64, 64)
- Cell dimensions: (dz, dy, dx) - typically (50m, 100m, 100m)
- Units: g/cm³

## Training Data Generation

Generate synthetic training data using `data_get.py` and `datagen.py`:

```python
from data_get import DensityDataset
import torch

# Create dataset
dataset = DensityDataset(
    max_nlayers=14,
    max_nintrusion=3,
    nzyx=(32, 64, 64),
    density_range=(0.0, 0.0005),
    anomly_density_range=(0.5, 1.0)
)

# Generate samples
for density_model, gravity_data in dataset:
    # Use for training
    pass
```

## GPU Acceleration

The code supports multi-GPU training using PyTorch's DistributedDataParallel:

```bash
# Multi-GPU training
python train.py --world_size 4 --device cuda:0
```

## Citation

If you use this code in your research, please cite:

```
[Citation information to be added - this appears to be research code]
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

[License information to be specified by the repository owner]

## Acknowledgments

- This project uses the [GeoIST](https://github.com/gravity-igpcea/geoist) library for geophysical computations
- Built with PyTorch for deep learning
- Inspired by research in potential field continuation methods

## Support

For questions or issues:
- Open an issue on GitHub
- Check the Jupyter notebooks for detailed examples
- Review the code documentation in each module

## Further Reading

### Downward Continuation in Geophysics

Downward continuation is used to:
- Enhance anomaly resolution from airborne or satellite gravity data
- Reduce observation height effects
- Improve geological interpretation
- Integrate multi-scale gravity surveys

### Deep Learning Approach

DC-Net advantages over traditional methods:
- Better noise handling through learned features
- Automatic feature extraction from training data
- No need for parameter tuning per dataset
- Faster computation for large datasets
- More stable continuation over large distances

## Version History

- **v1.0** - Initial release with core functionality
- Training code for encoder-decoder architecture
- Multiple example notebooks
- Support for synthetic and real data

---

**Note**: This repository contains both Chinese (original) and English (translated) versions of the Jupyter notebooks. The code and functionality are identical; only the documentation differs for accessibility.
