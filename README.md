# multimodal-crop-boundary

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
> Mehmet Furkan Celik*, Claudio Persello, Andrew Nelson, Giulia Conchedda, Francesco Tubiello, Claudia Paris  
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
