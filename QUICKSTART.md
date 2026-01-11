# DC-Net Quick Start Guide

This guide will help you get started with DC-Net for gravity field downward continuation.

## Prerequisites

Before starting, ensure you have:
- Python 3.7 or higher installed
- A GPU with CUDA support (recommended but not required)
- Basic understanding of Python and Jupyter notebooks

## Step 1: Installation

```bash
# Clone the repository
git clone https://github.com/RichardScottOZ/DC-Net-code.git
cd DC-Net-code

# Install dependencies
pip install torch numpy matplotlib pandas scipy jupyter
pip install geoist

# For GPU support (if available)
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```

## Step 2: Understanding the Workflow

The typical DC-Net workflow consists of three stages:

### Stage 1: Data Preparation
**Notebook**: `dataset_acquisition.ipynb` (获取数据集.ipynb)

This notebook demonstrates how to:
- Generate synthetic gravity data for training
- Create density models with various geological features
- Add realistic noise to simulate real-world conditions
- Prepare training datasets

**When to use**: Before training a new model or when you need custom training data.

### Stage 2: Model Training
**Script**: `train.py`

Train the DC-Net model:
```bash
python train.py \
  --checkpoint ./models/checkpoint.pt \
  --device cuda:0 \
  --epochs 600 \
  --batch_size 64
```

**Training time**: Approximately 2-4 hours on a modern GPU, depending on dataset size.

### Stage 3: Inference and Evaluation

#### Option A: Method Comparison
**Notebook**: `method_comparison.ipynb` (方法对比.ipynb)

- Compare DC-Net with traditional methods
- Evaluate accuracy and noise robustness
- Generate comparison plots

**Use this when**: You want to understand how DC-Net performs relative to classical techniques.

#### Option B: Testing on Synthetic Data
**Notebook**: `results_testing.ipynb` (结果测试.ipynb)

- Test trained model on synthetic test cases
- Validate model performance
- Check model generalization

**Use this when**: You want to verify model accuracy before applying to real data.

#### Option C: Real Data Processing
**Notebook**: `actual_data_processing.ipynb` (实际资料处理.ipynb)

- Process real gravity survey data
- Apply downward continuation
- Visualize and analyze results

**Use this when**: You have real gravity measurements to process.

#### Option D: Making Predictions
**Notebook**: `results_prediction.ipynb` (结果预测.ipynb)

- Use trained model for predictions
- Export to ONNX for deployment
- Batch processing workflows

**Use this when**: You need to process multiple datasets or deploy the model.

## Step 3: First Run - Testing with Pre-trained Model

If you have a pre-trained model checkpoint, start here:

```python
# In a Python script or Jupyter notebook
import torch
import numpy as np
import encoder
import decoder

# 1. Load the model
checkpoint = torch.load('./models/checkpointSH_noisefree_500m_adj.pt')
netEnc = encoder.GravEncoder()
netEnc.load_state_dict(checkpoint['enc_state_dict'])
netEnc.eval()

# 2. Prepare your data (64x64 grid)
gravity_data = np.load('your_gravity_data.npy')  # Shape: (64, 64)

# 3. Run inference
with torch.no_grad():
    input_tensor = torch.from_numpy(gravity_data).float()
    input_tensor = input_tensor.reshape(1, 1, 64, 64)
    output = netEnc(input_tensor)
    
# 4. Extract results
continued_gravity = output.numpy().squeeze()
```

## Step 4: Training Your Own Model

### 4.1 Generate Training Data

Open `dataset_acquisition.ipynb` and run all cells to generate training data.

### 4.2 Start Training

```bash
# Create models directory
mkdir -p models

# Start training
python train.py \
  --checkpoint ./models/my_model.pt \
  --device cuda:0 \
  --epochs 600 \
  --batch_size 64
```

Monitor training progress:
- Loss values should decrease over time
- Check TensorBoard logs (if configured)
- Save checkpoints periodically

### 4.3 Evaluate Your Model

Use `results_testing.ipynb` to evaluate your trained model on test data.

## Step 5: Processing Real Data

### 5.1 Data Preparation

Your gravity data should be:
- Gridded at regular intervals
- Interpolated to 64x64 grid size
- In units of mGal or SI units
- Stored as NumPy array

### 5.2 Apply Downward Continuation

Open `actual_data_processing.ipynb` and:
1. Load your gravity data
2. Specify the model checkpoint
3. Run the continuation
4. Visualize results

### 5.3 Iterate if Needed

You may need to:
- Adjust the continuation height
- Use different model checkpoints (with/without noise handling)
- Process data in overlapping windows if larger than 64x64

## Common Issues and Solutions

### Issue: "Module 'geoist' not found"
**Solution**: Install geoist with `pip install geoist`

### Issue: "CUDA out of memory"
**Solution**: 
- Reduce batch size: `--batch_size 32`
- Use CPU: `--device cpu`

### Issue: "Poor results on real data"
**Solution**:
- Ensure data is properly preprocessed and gridded
- Try model trained with noise: `checkpointSH_withnoise_500m.pt`
- Check that continuation height matches training

### Issue: "Notebooks have Chinese comments"
**Solution**: The English versions have the same functionality. Chinese comments in code cells are minimal and don't affect execution.

## Tips for Best Results

1. **Data Quality**: Ensure input data is properly gridded and free of major artifacts
2. **Model Selection**: Use noise-robust models for noisy field data
3. **Continuation Height**: Match the continuation distance to your model's training
4. **Validation**: Always validate results against known features or traditional methods
5. **Preprocessing**: Normalize or detrend data if necessary before processing

## Next Steps

- Explore all notebooks to understand different use cases
- Experiment with different model architectures in `encoder.py`, `decoder.py`
- Generate custom training data for your specific application
- Compare results with traditional continuation methods

## Getting Help

- Check the main [README.md](README.md) for detailed documentation
- Review notebook comments and code
- Open an issue on GitHub for bugs or questions

## Example Workflow

```bash
# 1. Setup
cd DC-Net-code
pip install torch numpy matplotlib pandas scipy jupyter geoist

# 2. Generate training data
jupyter notebook dataset_acquisition.ipynb

# 3. Train model
python train.py --checkpoint ./models/my_model.pt --epochs 600

# 4. Test model
jupyter notebook results_testing.ipynb

# 5. Process real data
jupyter notebook actual_data_processing.ipynb
```

That's it! You're ready to use DC-Net for gravity field downward continuation.
