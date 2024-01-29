# ArtCNN

## Overview
These are Super-Resolution Convolutional Neural Networks as GLSL shaders for mpv. They implement a simple feed-forward architecture with one long-skip connection and a pixel-shuffle layer to get the HR image.

![Model Architecture](./Images/model_architecture.png "Model Architecture")

The main variant of the shader is offered in 3 sizes:
- `ArtCNN_C4F32.glsl`: This has 4 internal convolution layers with 32 filters each. This is the "big" variant of the shader. This is the best version of the shader but it is a bit resource-intensive for real-time video playback,
you should only consider using it if you have a good GPU.
- `ArtCNN_C4F16.glsl`: This has 4 internal convolution layers with 16 filters each. This is the "normal" variant of the shader. Most semi-recent discrete GPUs should be able to handle this.
- `ArtCNN_C4F8.glsl`: This has 4 internal convolution layers with 8 filters each. This is the "small" variant of the shader. You should only use this for performance reasons.

A few other variants are also offered, these are meant to cover specific needs or edge-case scenarios:
- `ArtCNN_C4F16_D2.glsl`: Trained with JPEG LR images that have been moderately compressed. Use this when your objective is to get rid of artifacts.
- `ArtCNN_C4F16_D1.glsl`: Trained with JPEG LR images that have been lightly compressed. Use this for normal lossy web content, it preserves more fine detail than the D2 variant.
- `ArtCNN_C4F16_LL.glsl`: Trained with images downsampled in linear light. Use this if you suspect the content has been downsampled in linear light.

When in doubt of which variant to use, start with `ArtCNN_C4F16.glsl` to see if your system can handle it and go up or down from there.

The shaders are trained on the Manga109 dataset using the Adam optimiser with a learning rate of 1e-4 and the L1/MAE loss function. The high-resolution images are downsampled with a box filter, and they're also split into small 64x64 patches for performance and memory reasons.

You can check the `ArtCNN_Training.ipynb` Colab Notebook for details.

## Instructions
Add something like this to your mpv config:
```
vo=gpu-next
glsl-shader="path/to/shader/ArtCNN_C4F16.glsl"
```

## Parameters
You can set the following parameters:
- `ar_strength`: Controls the intensity limits of each HR pixel based on the LR pixels that surround it. If you plan on completely disabling AR, you can set `#define ANTIRING 0` to speed the last pass a little bit.

On `vo=gpu-next`, you can set these settings with `--glsl-shader-opts=param1=value1,param2=value2,...`.

## Example
![Example](./Images/example.png "Example")
