# ArtCNN

## Overview
These are Super-Resolution Convolutional Neural Networks as GLSL shaders for mpv. They implement a simple feed-forward architecture with one long-skip connection and a pixel-shuffle layer to get the HR image.

![Model Architecture](./Images/model_architecture.png "Model Architecture")

These shaders are trained on the Manga109 dataset using the Adam optimiser with a learning rate of 1e-4 and the L1/MAE loss function. The high-resolution images are downscaled in linear-light with a box filter, and they're also split into small 64x64 patches for performance and memory reasons.

You can check the `ArtCNN_Training.ipynb` Colab Notebook for details.

The main variant of the shader is offered in 3 sizes:
- `ArtCNN_C4F32.glsl`: This has 4 internal convolution layers with 32 filters each. This is the "big" variant of the shader.
- `ArtCNN_C4F16.glsl`: This has 4 internal convolution layers with 16 filters each. This is the "normal" variant of the shader.
- `ArtCNN_C4F8.glsl`: This has 4 internal convolution layers with 8 filters each. This is the "small" variant of the shader.

A few other variants are also offered, these are meant to cover specific needs or edge-case scenarios:
- `ArtCNN_C4F16_JPEG.glsl`: Trained with JPEG LR images. Use this when your source has visible compression artifacts (the model knows how to clean/ignore them to some extent).
- `ArtCNN_C4F8_Gamma.glsl`: Trained with images downsampled in gamma-light. Use this if you suspect the content has been downsampled in gamma-light.

When in doubt of which variant to use, start with `ArtCNN_C4F8.glsl` to see if your system can handle it and go up from there.

## Example
![Example](./Images/example.png "Example")

## Instructions
Add something like this to your mpv config:
```
vo=gpu-next
glsl-shader="path/to/shader/ArtCNN_C4F16.glsl"
```
