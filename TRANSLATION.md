# Notebook Translation Reference

This document provides a quick reference mapping between Chinese and English notebook names.

## Notebook Name Mapping

| Chinese Name (中文名称) | English Name | Purpose |
|------------------------|--------------|---------|
| 传统延拓方法.ipynb | traditional_continuation_method.ipynb | Traditional continuation methods comparison |
| 获取数据集.ipynb | dataset_acquisition.ipynb | Generate training datasets |
| 方法对比.ipynb | method_comparison.ipynb | Compare DC-Net with other methods |
| 结果测试.ipynb | results_testing.ipynb | Test trained models |
| 结果预测.ipynb | results_prediction.ipynb | Make predictions with trained models |
| 实际资料处理.ipynb | actual_data_processing.ipynb | Process real gravity data |

## Usage

Both Chinese and English versions of the notebooks contain identical code and functionality. The only difference is the language used in any documentation or comments.

### If you read Chinese
Use either version - they are functionally identical.

### If you read English
Use the English versions (those with underscores in filenames).

## Technical Details

- **Content**: All notebooks contain the same Python code
- **Comments**: Minimal Chinese comments in code cells have been translated
- **Functionality**: 100% identical between language versions
- **Data**: Both versions work with the same data files
- **Models**: Both versions use the same model checkpoints

## Quick Reference

To convert a Chinese filename to English, use this mapping:

```python
translation_map = {
    '传统延拓方法': 'traditional_continuation_method',
    '获取数据集': 'dataset_acquisition',
    '方法对比': 'method_comparison',
    '结果测试': 'results_testing',
    '结果预测': 'results_prediction',
    '实际资料处理': 'actual_data_processing'
}
```

## File Organization

```
DC-Net-code/
├── Traditional Methods
│   ├── 传统延拓方法.ipynb (Chinese)
│   └── traditional_continuation_method.ipynb (English)
├── Data Preparation
│   ├── 获取数据集.ipynb (Chinese)
│   └── dataset_acquisition.ipynb (English)
├── Evaluation
│   ├── 方法对比.ipynb (Chinese)
│   ├── method_comparison.ipynb (English)
│   ├── 结果测试.ipynb (Chinese)
│   └── results_testing.ipynb (English)
└── Application
    ├── 结果预测.ipynb (Chinese)
    ├── results_prediction.ipynb (English)
    ├── 实际资料处理.ipynb (Chinese)
    └── actual_data_processing.ipynb (English)
```

## Recommendations

- **New users**: Start with the English versions and follow the [QUICKSTART.md](QUICKSTART.md) guide
- **Chinese speakers**: Either version works perfectly
- **Documentation**: Refer to [NOTEBOOKS.md](NOTEBOOKS.md) for detailed descriptions in English
- **Collaboration**: When sharing notebooks, specify which version you're using

## Note

The Chinese notebooks are the original versions created by the authors. The English versions are translations provided for broader accessibility and do not modify any core functionality or code logic.
