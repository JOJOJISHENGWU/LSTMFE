# LSTMFE




### 快速开始 (3步即可复现)

```bash
# 1. 安装依赖
pip install -r requirements.txt

# 2. 一键复现最佳结果 (推荐PEMS07数据集)
python reproduce_best_results.py PEMS07

# 3. 验证结果
python test.py
```

## 📊 期望结果

| 数据集 | 最佳结果 MAE | 复现范围 MAE | 最佳结果 MAPE | 复现范围 MAPE |
|--------|-------------|-------------|--------------|-------------|
| PEMS07 | 16.63 | 16.0-17.5 | 7.79% | 7.5-8.5% |
| PEMS08 | 12.47 | 12.0-13.5 | 9.04% | 8.5-10.0% |
| PEMS03 | TBD | TBD | TBD% | TBD% |
| PEMS04 | TBD | TBD | TBD% | TBD% |

**📝 说明**: 
- **最佳结果**: 作者多次训练得到的最优值
- **复现范围**: 考虑训练随机性的合理期望范围
- 如果您的结果在范围内，说明复现成功
- 如果偏差较大，建议运行2-3次取最佳结果

## 📁 数据集准备

确保数据集按以下结构放置：

```
项目根目录/
├── PEMS03/
│   ├── PEMS03.npz        # 交通流数据
│   └── adj.npy           # 邻接矩阵 (可选，缺失时自动生成)
├── PEMS04/
│   ├── PEMS04.npz
│   └── adj.npy
├── PEMS07/
│   ├── PEMS07.npz
│   └── adj.npy
├── PEMS08/
│   ├── PEMS08.npz
│   └── adj.npy
└── ...
```

## 🔧 环境要求

- **Python**: >= 3.7
- **PyTorch**: >= 1.9.0
- **CUDA**: 建议使用GPU加速 (可选)

## 🚀 使用方法

### 方法1: 一键复现 (推荐)

```bash
# 使用最佳配置训练指定数据集
python reproduce_best_results.py PEMS07

# 或者让脚本自动选择可用数据集
python reproduce_best_results.py
```

### 方法2: 自定义训练

```bash
# 基础训练
python train.py

# 测试已训练模型
python test.py
```

### 方法3: 交互式训练

```python
from train import train_and_evaluate
from best_config import get_best_config

# 获取最佳配置
config = get_best_config('PEMS07')

# 训练模型
results = train_and_evaluate(
    dataset='PEMS07',
    **{k: v for k, v in config.items() if k != 'expected_results'}
)

print(f"MAE: {results['MAE']:.4f}")
```

## 📈 模型架构

本模型包含三个核心模块：

1. **Module 1: Multiscale Temporal Feature Extraction (MTFE)**
   - 基于EinFFT的多尺度时间特征提取
   - 自动周期检测和频域增强

2. **Module 2: Graph-Enhanced Coordinate Attention (GECA)**
   - 切比雪夫图卷积网络
   - 坐标注意力机制

3. **Module 3: Time-aware Decomposition (TATSD)**
   - 时间感知的趋势季节分解
   - 自适应权重学习

## 🎛️ 关键超参数

```python
BEST_PARAMS = {
    'd_model': 96,           # 模型维度
    'decoder_layers': 3,     # 解码器层数
    'seq_len': 12,          # 输入序列长度
    'batch_size': 32,       # 批次大小
    'epochs': 300,          # 训练轮数
    'learning_rate': 1e-3,  # 学习率
    'top_k': 3,             # 主要频率数量
    'use_decomposition': True,    # 是否使用分解模块
    'decomp_weight': 0.1,   # 分解模块权重
}
```

## 🔍 故障排除

### 常见问题

1. **数据集未找到**
   ```
   解决方案: 确保数据集文件名和路径正确
   ```

2. **CUDA内存不足**
   ```bash
   # 减小批次大小
   python reproduce_best_results.py PEMS07 --batch_size 16
   ```

3. **结果差异较大**
   ```
   可能原因: 
   - 硬件差异导致的数值精度差异
   - PyTorch版本不同
   - 随机种子影响
   ```

### 性能优化

- **GPU加速**: 自动检测并使用CUDA
- **混合精度**: 可在代码中启用AMP加速训练
- **批次大小**: 根据显存调整batch_size

## 📊 实验复现

### 完整复现流程

1. **环境设置**
   ```bash
   conda create -n st-mtfe python=3.8
   conda activate st-mtfe
   pip install -r requirements.txt
   ```

2. **数据准备** 
   - 下载PEMS数据集
   - 放置在正确目录结构中

3. **训练模型**
   ```bash
   python reproduce_best_results.py PEMS07
   ```

4. **验证结果**
   ```bash
   python test.py
   ```

### 预期训练时间

| 数据集 | GPU (V100) | CPU | 内存使用 |
|--------|------------|-----|----------|
| PEMS03 | ~2小时     | ~8小时 | ~4GB |
| PEMS04 | ~3小时     | ~12小时 | ~6GB |
| PEMS07 | ~2.5小时   | ~10小时 | ~5GB |
| PEMS08 | ~3.5小时   | ~14小时 | ~7GB |

## 📝 引用

如果这个代码对您的研究有帮助，请引用我们的论文：

```bibtex
@article{your_paper_2024,
  title={Your Paper Title},
  author={Your Name},
  journal={Your Journal},
  year={2024}
}
```

## 📞 联系方式

如有问题，请：
- 提交Issue到GitHub仓库
- 发送邮件至：your.email@example.com

## 🙏 致谢

感谢所有为此项目做出贡献的研究者和开发者。

---
**最后更新**: 2024年7月30日
Dataset link: https://github.com/HITPLZ/DSTRformer
