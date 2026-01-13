# Learned Image Compression with Perceptual Quality at Matched Bitrates
This project builds and studies a small neural-network image compressor that learns how to trade off file size, image quality, and human-perceived visual quality.
Instead of using fixed rules like JPEG or WebP, the model learns how to compress images directly from data using deep learning.

## What this project does
The system trains a convolutional autoencoder to turn an image into a compact latent code, then reconstructs it. A learned entropy model estimates how many bits are needed to store that code, so the model learns to make images small while simultaneously emphasizing visual quality. The training objective balances three things: how small the file is (bitrate), how close the pixels are (MSE/PSNR), and the visual quality as it relates to human perception (LPIPS, MS-SSIM). This lets the model operate in the very-low-bitrate regime where traditional codecs struggle.

## What is inside
| File                       | Purpose                                               |
| -------------------------- | ----------------------------------------------------- |
| `imagecompression.py`   | Full training, model, and evaluation code             |
| `Final_Report.pdf` | Final paper with experiments, tables, and comparisons |

## Model design

The compressor has:

Encoder → Quantizer → Entropy model → Decoder

The encoder shrinks the image into a latent space.
The quantizer makes it discrete like real compression.
The entropy model predicts how many bits it needs.
The decoder rebuilds the image.

A learned per-channel quantization scale lets the network control how finely each feature is stored.

Two entropy models are tested:

* Factorized Gaussian prior*
* Hyperprior (with side information)

The simpler factorized model works better for this small network.

## Loss function

The model is trained with:

Loss = λ · Bitrate + MSE + β · (1 − MS-SSIM) + γ · LPIPS

This lets us tune how much we care about:

* Size
* Pixel accuracy
* Structural similarity
* Perceptual realism

## Experimental results

From the experiments:

* MSE-only training gives the best baseline
* MS-SSIM hurts performance in this small model
* A small LPIPS weight improves visual quality with little bitrate cost
* Factorized prior beats hyperprior here
* The learned model is stronger than JPEG/WebP at very low bitrates
* JPEG and WebP still win when you allow more bits

The best learned model reaches about 0.2 bits per pixel with strong perceptual quality, which is far smaller than JPEG or WebP at similar visual quality.

## Why this matters

Modern codecs are hand-engineered.
This project shows that even a small neural compressor can learn to:

* Preserve edges
* Reduce blocking
* Keep textures natural

at extremely low bitrates.

This is important for:

* Streaming
* Mobile cameras
* Edge devices
* Storage systems
* Low-bandwidth AI pipelines
