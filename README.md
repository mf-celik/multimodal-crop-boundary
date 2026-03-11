# multimodal-crop-boundary

# 🚧 Under Construction
This repository is currently under active development. Features, structure, and documentation may change.

## Overview
This repository contains the implementation of a **dual-stream U-Net architecture** for accurate crop field boundary mapping using multimodal Sentinel-1 (SAR) and Sentinel-2 (optical) satellite data. The approach leverages spatial-channel attention mechanisms (scSE blocks) and composite edge-aware loss functions to produce sharp, continuous, and topologically consistent field boundaries.
The method is particularly effective for **small, fragmented agricultural fields** typical of smallholder farms in Mainland Southeast Asia.

## Key Features
✅ **Multimodal Fusion**: Combines multitemporal Sentinel-1 VH backscatter with Sentinel-2 monthly composites  
✅ **Edge-Aware Learning**: Composite loss function (BCE + Tversky + Sobel edge penalty) for sharp boundary delineation  
✅ **Spatial-Channel Attention**: scSE blocks for adaptive feature recalibration  
✅ **Early-Mid-Late Level Fusion Strategy**: Modality-specific encoders and decoders
✅ **Production Ready**: Includes training, validation and inference pipelines  
✅ **Extensible Dataset**: Extended AI4SmallFarms benchmark with coregistered Sentinel-1 data (available on Zenodo https://doi.org/10.5281/zenodo.18921642)

## Paper
> **Edge-Aware Crop Field Boundary Mapping from Multimodal Sentinel-1 and Sentinel-2 Data**  
> Mehmet Furkan Celik, Claudio Persello, Andrew Nelson, Giulia Conchedda, Francesco Tubiello, Claudia Paris  
> IEEE Geoscience and Remote Sensing Letters, 2025

Citation:
```bibtex
@article{celik2025edge,
  title={Edge-Aware Crop Field Boundary Mapping from Multimodal Sentinel-1 and Sentinel-2 Data},
  author={Celik, Mehmet Furkan and Persello, Claudio and Nelson, Andrew and Conchedda, Giulia and Tubiello, Francesco and Paris, Claudia},
  journal={IEEE Geoscience and Remote Sensing Letters},
  year={2025}
}
```
## Architecture Overview
<div align="center">
    <img src="figures/fusion_types.png" alt="Fusion strategies within the model" width="700"/>
</div>

Illustrates the fusion strategies used within the model architecture. In general, these strategies differ in the stage at which information from multiple modalities is combined. 
Early-level fusion integrates modality-specific features at the input stage, simply concatanete images, allowing the model to learn joint representations from the beginning. 
Late-level fusion combines modality-specific predictions only after independent processing, which preserves specialized encoders and decoders but limit cross-modal interaction. 
Mid-level fusion lies between these two strategies, enabling information fusion at bottleneck and often providing a balance between flexibility and representational power.

## Installation
```bash
pip install -r requirements.txt
```

Or using conda:
```bash
conda create -n crop-boundary python=3.10
conda activate crop-boundary
pip install -r requirements.txt
```

### Prerequisites

### Clone Repository

## Dataset

### AI4SmallFarms Benchmark
Download the AI4SmallFarms dataset from the official repository: 
```
https://doi.org/10.17026/DANS-XY6-NGG6
```

### Extended Sentinel-1 Data
Multitemporal Sentinel-1 VH backscatter coregistered with the existing Sentinel-2 data is available on Zenodo:
```
https://doi.org/10.5281/zenodo.18921642
```

Download and place the data in the `data/` directory:
```
data/
├── S2/
├── S1/
├── train_tiles/
├── val_tiles/
└── test_tiles/
```

## Quick Start

### 1. Training

### 2. Inference

### 3. Post-Processing

### 4. Evaluation

## Model Architecture
### Dual-Stream U-Net with scSE Blocks

**Encoder Path (Dual-Stream)**
- Two parallel encoder streams process S1 and S2 independently
- Each stream: Conv → BatchNorm → ReLU → scSE → MaxPool (4 stages)
- scSE blocks enable spatial and channel attention
- Progressive downsampling with receptive field expansion

**Decoder Path (Shared)**
- Single shared decoder upsamples fused features
- Skip connections concatenate encoder features at each scale
- Transposed convolutions for upsampling
- scSE blocks for adaptive feature recalibration
- Final refinement stage before output

**Output Head**
- Point-wise convolution → Sigmoid activation
- Single-channel boundary probability map (0-1)

<div align="center">
    <img src="figures/mid_level_fusion_sketch.png" alt="dual-stream U-Net architecture for accurate crop boundary delineation with Sentinel-2 & Sentinel-1 images" height="px"/>
</div>

### Spatial-Channel Squeeze-Excitation (scSE)
```
Input Feature Map (C, H, W)
        |
    ┌───┴───┐
    ▼       ▼
 Spatial  Channel
   SE      SE
    |       |
    └───┬───┘
        ▼
  Recalibrated Features
```

## Loss Function

The model is trained with a composite edge-aware loss:

```
L_total = λ·L_BCE + λ·L_Tversky + λ·L_Sobel
```

- **Binary Cross-Entropy (BCE)**: Pixel-wise classification accuracy
- **Tversky Loss**: Class imbalance handling with asymmetric precision-recall weighting
- **Sobel Edge Loss**: Gradient-based boundary sharpness and continuity penalty

These three components balance region accuracy, robustness to imbalance, and boundary precision without over-parameterization (weights fixed to 1).
