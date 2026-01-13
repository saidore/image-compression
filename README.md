# image-compression
The system trains a convolutional autoencoder to turn an image into a compact latent code, then reconstructs it. A learned entropy model estimates how many bits are needed to store that code, so the model learns to make images small while simultaneously emphasizing visual quality.
