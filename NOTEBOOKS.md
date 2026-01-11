# Jupyter Notebook Documentation

This document provides detailed information about each Jupyter notebook in the DC-Net repository. All notebooks are available in both Chinese (original) and English (translated) versions.

## Notebook Overview

| English Name | Chinese Name | Purpose | Difficulty |
|-------------|--------------|---------|------------|
| traditional_continuation_method.ipynb | 传统延拓方法.ipynb | Compare traditional methods | Intermediate |
| dataset_acquisition.ipynb | 获取数据集.ipynb | Generate training data | Beginner |
| method_comparison.ipynb | 方法对比.ipynb | Benchmark DC-Net | Intermediate |
| results_testing.ipynb | 结果测试.ipynb | Test trained models | Beginner |
| results_prediction.ipynb | 结果预测.ipynb | Make predictions | Intermediate |
| actual_data_processing.ipynb | 实际资料处理.ipynb | Process real data | Advanced |

---

## 1. traditional_continuation_method.ipynb

**Chinese**: 传统延拓方法.ipynb

### Purpose
Demonstrates traditional downward continuation methods and compares them with DC-Net's deep learning approach.

### What You'll Learn
- How classical downward continuation works
- Mathematical principles of potential field transformation
- Limitations of traditional methods with noisy data
- Advantages of the deep learning approach

### Key Operations
1. Generate synthetic gravity data from a density model
2. Apply traditional continuation methods
3. Compare accuracy and noise sensitivity
4. Visualize differences between methods

### Prerequisites
- Basic understanding of gravity field theory
- Familiarity with NumPy and Matplotlib
- Understanding of forward gravity modeling

### Expected Runtime
~2-5 minutes (depends on model complexity)

### Key Outputs
- Comparison plots showing traditional vs DC-Net results
- Quantitative error metrics
- Visualization of continuation at different heights

---

## 2. dataset_acquisition.ipynb

**Chinese**: 获取数据集.ipynb

### Purpose
Generate synthetic training datasets for DC-Net model training.

### What You'll Learn
- How to create realistic density models
- Forward gravity modeling techniques
- Adding noise to simulate real-world conditions
- Dataset preparation and formatting

### Key Operations
1. Define density model parameters (layers, intrusions)
2. Generate 3D density distributions
3. Compute forward gravity response
4. Add various noise levels
5. Save training data in appropriate format

### Prerequisites
- Basic understanding of gravity forward modeling
- Familiarity with the GeoIST library
- Understanding of neural network training data requirements

### Expected Runtime
~10-30 minutes (depending on dataset size)

### Key Parameters
- `data_length`: Number of training samples (default: 50000)
- `nzyx`: Model dimensions (32, 64, 64)
- `dzyx`: Cell sizes (50m, 100m, 100m)
- `max_nlayers`: Maximum geological layers
- `max_nintrusion`: Maximum intrusion bodies

### Key Outputs
- Training dataset files (.npy format)
- Validation dataset files
- Visualization of sample models

---

## 3. method_comparison.ipynb

**Chinese**: 方法对比.ipynb

### Purpose
Comprehensive comparison between DC-Net and traditional downward continuation methods.

### What You'll Learn
- Quantitative evaluation of continuation methods
- Performance under various noise conditions
- Computational efficiency comparisons
- When to use DC-Net vs traditional methods

### Key Operations
1. Load test datasets with known ground truth
2. Apply DC-Net continuation
3. Apply traditional methods (e.g., FFT-based)
4. Compute error metrics (RMSE, correlation, etc.)
5. Generate comparison visualizations

### Prerequisites
- Trained DC-Net model checkpoint
- Understanding of continuation methods
- Basic statistics knowledge

### Expected Runtime
~5-10 minutes

### Key Metrics Computed
- Root Mean Square Error (RMSE)
- Correlation coefficients
- Peak signal-to-noise ratio
- Computation time

### Key Outputs
- Side-by-side comparison plots
- Error distribution maps
- Quantitative performance tables
- Noise robustness analysis

---

## 4. results_testing.ipynb

**Chinese**: 结果测试.ipynb

### Purpose
Test and validate trained DC-Net models on synthetic test cases.

### What You'll Learn
- How to load and use trained models
- Model validation techniques
- Interpreting test results
- Identifying model limitations

### Key Operations
1. Load model checkpoint
2. Load test datasets
3. Run inference on test data
4. Compute validation metrics
5. Visualize predictions vs ground truth

### Prerequisites
- Trained model checkpoint file
- Test datasets
- Basic understanding of model evaluation

### Expected Runtime
~2-5 minutes

### Key Parameters
- `checkpoint`: Path to model file (e.g., './models/checkpoint.pt')
- Test data directory

### Key Outputs
- Prediction accuracy metrics
- Visual comparison of predicted vs actual
- Model performance summary

---

## 5. results_prediction.ipynb

**Chinese**: 结果预测.ipynb

### Purpose
Use trained DC-Net models to make predictions on new data and export models for deployment.

### What You'll Learn
- Production inference workflow
- Model export to ONNX format
- Batch processing techniques
- Result interpretation

### Key Operations
1. Load trained model
2. Prepare input data
3. Run batch predictions
4. Export model to ONNX (for deployment)
5. Visualize and save results

### Prerequisites
- Trained model checkpoint
- Input gravity data files
- Basic understanding of model deployment

### Expected Runtime
~5-10 minutes (depending on batch size)

### Key Features
- ONNX export for cross-platform deployment
- Batch processing support
- Customizable output formats
- Integration with visualization tools

### Key Outputs
- Predicted gravity fields at target altitude
- Exported ONNX model file
- Visualization of results
- Saved prediction arrays

---

## 6. actual_data_processing.ipynb

**Chinese**: 实际资料处理.ipynb

### Purpose
Complete workflow for processing real gravity survey data using DC-Net.

### What You'll Learn
- Real data preprocessing steps
- Applying DC-Net to field measurements
- Result interpretation and validation
- Handling real-world data challenges

### Key Operations
1. Load real gravity survey data
2. Preprocess and grid the data
3. Normalize and format for DC-Net
4. Apply downward continuation
5. Post-process and visualize results
6. Validate against known geological features

### Prerequisites
- Real gravity survey data
- Trained model (preferably with noise robustness)
- Understanding of survey data formats
- Geological context of survey area

### Expected Runtime
~10-20 minutes

### Data Requirements
- Gravity data in standard format (e.g., .dat, .gdf)
- Spatial coordinates (X, Y)
- Data quality indicators
- Known measurement altitude

### Processing Steps

#### Step 1: Data Loading
```python
# Load gravity data
data = np.loadtxt("survey-data.dat").T
X, Y, gravity = data[0], data[1], data[2]
```

#### Step 2: Preprocessing
- Remove regional trends
- Grid to regular spacing
- Interpolate to 64x64 grid
- Normalize values

#### Step 3: DC-Net Application
- Load appropriate model checkpoint
- Apply continuation
- Handle edge effects

#### Step 4: Visualization
- Generate contour maps
- Compare with upward continued data
- Overlay geological features

### Key Outputs
- Continued gravity field at target altitude
- Quality control plots
- Comparison with traditional methods
- Interpretation-ready visualizations

### Tips for Real Data
1. **Quality Control**: Remove outliers before processing
2. **Grid Size**: Interpolate carefully to 64x64 without introducing artifacts
3. **Model Selection**: Use noise-robust models for field data
4. **Validation**: Compare with known features or independent data
5. **Iteration**: May need to process overlapping windows for large surveys

---

## Running the Notebooks

### Starting Jupyter

```bash
cd DC-Net-code
jupyter notebook
```

This will open Jupyter in your browser. Navigate to any notebook and click to open.

### Execution Order

For a complete workflow, run notebooks in this order:

1. **First Time Setup**
   - `dataset_acquisition.ipynb` - Generate training data
   - Train model using `train.py`

2. **Model Validation**
   - `results_testing.ipynb` - Validate trained model
   - `method_comparison.ipynb` - Benchmark performance

3. **Production Use**
   - `actual_data_processing.ipynb` - Process real data
   - `results_prediction.ipynb` - Batch predictions

### Notebook Cells

All notebooks use standard Jupyter cell execution:
- **Shift + Enter**: Run current cell and move to next
- **Ctrl + Enter**: Run current cell
- **Cell → Run All**: Execute entire notebook

### Troubleshooting

#### Kernel Errors
If the kernel dies or becomes unresponsive:
1. Restart kernel: Kernel → Restart
2. Clear outputs: Cell → All Output → Clear
3. Run cells again from the beginning

#### Import Errors
Missing modules can be installed from within notebooks:
```python
!pip install package_name
```

#### Memory Issues
For large datasets:
- Reduce batch size
- Process data in chunks
- Use a machine with more RAM
- Close other notebooks

---

## Customization

### Modifying Notebooks

All notebooks can be customized for your needs:

1. **Parameters**: Change at the top of notebooks
2. **Visualizations**: Modify matplotlib code
3. **Processing**: Adjust preprocessing steps
4. **Output**: Change save locations and formats

### Creating New Notebooks

To create a workflow for your specific case:
1. Copy an existing notebook as a template
2. Modify data loading section
3. Adjust preprocessing for your data format
4. Customize visualization and output

### Batch Processing

For processing multiple files:
```python
import glob

# Process all .dat files
for file in glob.glob("data/*.dat"):
    # Load data
    data = np.loadtxt(file)
    # Process with DC-Net
    result = process_with_dcnet(data)
    # Save result
    np.save(f"results/{file}_result.npy", result)
```

---

## Best Practices

1. **Version Control**: Keep original notebooks unchanged; create copies for experiments
2. **Documentation**: Add markdown cells explaining your modifications
3. **Data Management**: Keep data files organized in separate directories
4. **Checkpoints**: Save notebook regularly to avoid losing work
5. **Testing**: Run on small datasets first before full-scale processing

---

## Additional Resources

- **Main Documentation**: See [README.md](README.md)
- **Quick Start**: See [QUICKSTART.md](QUICKSTART.md)
- **Code Documentation**: Check docstrings in Python modules
- **GeoIST Library**: https://github.com/gravity-igpcea/geoist

---

## Contributing

If you create useful notebooks or improvements:
1. Document your changes clearly
2. Test on multiple datasets
3. Submit a pull request
4. Share your use case

## Support

For notebook-specific issues:
- Check cell outputs for error messages
- Verify all dependencies are installed
- Ensure data paths are correct
- Review prerequisites for each notebook

For general help, see the main README or open a GitHub issue.
