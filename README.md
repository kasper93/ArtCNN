# ArtCNN

## Overview
These are Super-Resolution Convolutional Neural Networks as GLSL shaders for mpv. They implement a simple feed-forward architecture with one long-skip connection and a pixel-shuffle layer to get the HR image.

Two variants of the shader are available:
- `ArtCNN_C4F16.glsl`: This is the main variant with 4 internal convolution layers with 16 filters each.
- `ArtCNN_C4F8.glsl`: This is a smaller and faster variant with only 8 filters per convolution layer.

## Implementation Details
These shaders are trained on the Manga109 dataset using the Adam optimiser with a learning rate of 1e-4 and the L1/MAE loss function. The high-resolution images are downscaled in linear-light with a box filter, and they're also split into small 64x64 patches for performance and memory reasons.

You can check the `ArtCNN_Training.ipynb` Colab Notebook for details.

## Model Architecture
![Model Architecture](./Images/model_architecture.png "Model Architecture")

## Example
![Example](./Images/example.gif "Example")

## Instructions
Add something like this to your mpv config:
```
glsl-shader="path/to/shader/ArtCNN_C4F16.glsl"
```
