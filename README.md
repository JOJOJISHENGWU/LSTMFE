# LSTMFE
A state-of-the-art deep learning framework for traffic flow prediction, combining coordinate attention,Ein-FFT, and temporal decomposition techniques.

## 🎯 Overview

LSTMFE is an advanced model designed for accurate traffic flow prediction. The model integrates multiple cutting-edge techniques to capture both spatial dependencies between traffic sensors and temporal patterns in traffic data.

## ✨ Key Features

- **🔍 Coordinate Attention**: Enhanced spatial feature representation
- **🌐 Graph Convolution**: Efficient processing of sensor network topology  
- **⏰ Temporal Decomposition**: Separation of trend and seasonal components
- **🔄 Multiscale Extraction**: Feature learning at different temporal scales
- **⚡ GPU Acceleration**: Optimized for high-performance computing

## 🏗️ Architecture

### Core Components

1. **Coordinate Attention Mechanism**
   - Captures spatial position information
   - Enhances channel relationships
   - Improves long-range dependency modeling

2. **Chebyshev Graph Convolution**
   - Processes spatial relationships between sensors
   - Efficient polynomial-based graph operations
   - Scalable to large sensor networks

3. **Temporal Decomposition Module**
   - Separates trend and seasonal components
   - Adaptive pattern recognition
   - Improved temporal modeling

4. **Multiscale Feature Extractor**
   - Frequency domain transformations
   - Multiple temporal scale analysis
   - Enhanced pattern recognition

5. **Gating Mechanism**
   - Information flow control
   - Gradient vanishing prevention
   - Component integration

## 📊 Supported Datasets

| Dataset | Sensors | Time Steps | Features | Description |
|---------|---------|------------|----------|-------------|
| PEMS03 | 358 | 26,208 | 3 | San Francisco Bay Area |
| PEMS04 | 307 | 16,992 | 3 | San Francisco Bay Area |
| PEMS07 | 883 | 28,224 | 3 | Los Angeles County |
| PEMS08 | 170 | 17,856 | 3 | San Bernardino Area |

### Data Format
- **Features**: Flow, Occupancy, Speed
- **Temporal Resolution**: 5-minute intervals
- **Spatial Structure**: Highway sensor networks

## 🚀 Quick Start

### Prerequisites

```bash
Python >= 3.7
PyTorch >= 1.9.0
NumPy >= 1.19.0
```

### Installation

```bash
# Clone repository
git clone <repository-url>
cd stmodel

# Install dependencies
pip install torch numpy pandas scikit-learn

# Verify installation
python architecture.py
```

### Basic Usage

```python
from architecture import STModel, get_model_config

# Initialize model
config = get_model_config('PEMS08')
model = STModel(**config)

# Model summary
print(f"Parameters: {sum(p.numel() for p in model.parameters()):,}")
```

## ⚙️ Configuration

### Model Parameters

```python
MODEL_CONFIG = {
    'seq_len': 12,              # Input sequence length
    'num_nodes': 170,           # Number of sensors
    'd_model': 96,              # Hidden dimension
    'decoder_layers': 3,        # Decoder depth
    'use_decomposition': True,  # Enable decomposition
    'decomp_weight': 0.1,       # Decomposition loss weight
}
```

### Training Parameters

```python
TRAINING_CONFIG = {
    'batch_size': 32,           # Batch size
    'learning_rate': 0.001,     # Learning rate
    'epochs': 300,              # Training epochs
    'optimizer': 'Adam',        # Optimizer type
}
```

## 📈 Performance

### Evaluation Metrics

- **MAE**: Mean Absolute Error
- **MAPE**: Mean Absolute Percentage Error  
- **RMSE**: Root Mean Square Error


## 🔧 Advanced Features

### Multi-Dataset Support

```python
# Automatic configuration for different datasets
config_03 = get_model_config('PEMS03')  # 358 sensors
config_07 = get_model_config('PEMS07')  # 883 sensors
config_08 = get_model_config('PEMS08')  # 170 sensors
```

### GPU Optimization

```python
import torch

# Automatic device selection
device = torch.device('cuda' if torch.cuda.is_available() else 'cpu')
model = model.to(device)

# Mixed precision training support
from torch.cuda.amp import GradScaler, autocast
scaler = GradScaler()
```

### Memory Efficiency

- Gradient checkpointing for large models
- Batch size adaptation for different GPU memory
- Efficient data loading with multi-processing

## 🛠️ Development

### Architecture Extension

The modular design allows easy extension:

```python
# Add custom attention mechanism
class CustomAttention(nn.Module):
    def __init__(self, d_model):
        super().__init__()
        # Custom implementation
        pass

# Integrate into STModel
model.coordinate_attention = CustomAttention(d_model)
```

### Custom Loss Functions

```python
def custom_loss(y_pred, y_true, decomp_loss=0):
    mae_loss = torch.mean(torch.abs(y_pred - y_true))
    return mae_loss + 0.1 * decomp_loss
```

## 📋 System Requirements

### Recommended Setup
- **RAM**: 16GB+
- **GPU**: NVIDIA GPU with 8GB+ VRAM
- **Storage**: SSD with 10GB+ free space
- **CUDA**: 11.0+

## 🔍 Troubleshooting

### Common Issues

**Memory Error**
```bash
# Reduce batch size
batch_size = 16  # Default: 32
```

**CUDA Not Available**
```bash
# Install CUDA-enabled PyTorch
pip install torch --index-url https://download.pytorch.org/whl/cu118
```

**Dataset Not Found**
```bash
# Verify data structure
ls PEMS08/
# Should contain: PEMS08.npz, adj.npy
```

## 📚 Documentation

### API Reference

- `STModel`: Main architecture class
- `CoordinateAttention`: Spatial attention mechanism
- `ChebGraphConv`: Graph convolution layer
- `TemporalDecomposition`: Time series decomposition
- `MultiScaleFeatureExtractor`: Multi-scale feature learning

### Configuration Guide

- Model hyperparameters
- Dataset-specific settings
- Training optimization
- Hardware acceleration


### Development Setup

```bash
# Development installation
pip install -e .


## Data Preparation
LSTMFE is implemented on several public traffic datasets.  

### Datasets Used
- PEMS03, PEMS04, PEMS07 and PEMS08 from [STSGCN (AAAI-20)](https://github.com/Davidham3/STSGCN).  

