# ArtCNN

## Overview
These are Super-Resolution Convolutional Neural Networks as GLSL shaders for mpv. They implement a simple feed-forward architecture with one long-skip connection and a pixel-shuffle layer to get the HR image.

![Model Architecture](./Images/model_architecture.png "Model Architecture")

The main variant of the shader is offered in 2 sizes:
- `ArtCNN_C4F32.glsl`: This has 4 internal convolution layers with 32 filters each. This is the "big" variant of the shader. This is the best version of the shader for very high quality content, but it is a bit resource-intensive for real-time video playback and you should only consider using it if you have a good GPU.
- `ArtCNN_C4F16.glsl`: This has 4 internal convolution layers with 16 filters each. This is the "normal" variant of the shader. Most semi-recent discrete GPUs should be able to handle this.

A few other variants of `C4F16` are also offered, these are meant to cover specific needs:
- `ArtCNN_C4F16_LL.glsl`: Trained with images downsampled in linear light. Use this if you suspect the content has been downsampled in linear light.
- `ArtCNN_C4F16_SH.glsl`: Trained with LR images that have been downsampled with Hermite. Use this if you want some mild sharpening.
- `ArtCNN_C4F16_DN.glsl`: Trained with JPEG LR images that have been moderately compressed. Use this to clean compression artifacts at the expense of some fine-detail.
- `ArtCNN_C4F16_DS.glsl`: Trained with JPEG LR images that have been moderately compressed and downsampled with Hermite. This provides artifact cleaning and sharpening, which might work well for low quality web sources.

If you plan on using ArtCNN for fractional scaling factors below 2x, it's recommended that you change the default `dscale` from `hermite` to a sharper option to avoid blurring the image.

## Technical Details
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
- `ar_strength`: Controls the intensity limits of each HR pixel based on the LR pixels that surround it.

On `vo=gpu-next`, you can set these settings with `--glsl-shader-opts=ar_strength=value`.

## Example
![Example](./Images/example.png "Example")
