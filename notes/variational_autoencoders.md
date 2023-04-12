# Variational Autoencoders

## Normal Autoencoders

Normal autoencoders takes some kind of input data with a very high dimensionality, runs it through a neural network and reduces its dimensionality. It's made of encoder which takes input and lowers its dimensions into a bottleneck, and decoder tries to reconstruct the network. Comparing input and output images pixel to pixel we can get loss function which we can use to compress images. Fully connected layers can be replaced with CNNs.

4:50
This can be used to denoise images or reconstruct missing part of an images.

## Variational Autoencoders

Instead of mapping any input to a fixed vector, we want to map it to a distribution. Bottleneck vector is replaces by two vectors: mean vector and standard deviation vector.

Loss functions consists two parts: reconstruction loss and Dkl divergence. 

After bottleneck there is a sampled latent vector, which it takes a sample from distribution and feeds that sample to decoder. 
Reparametrization trick.



