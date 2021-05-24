---
title: "Replicating kernels with a short stride allows sparse reconstructions with fewer independent kernels"
collection: publications
permalink: /publication/2014-06-17-replicating-kernels
excerpt: 'Exploring the tradeoff of patch size, stride, and overcompleteness in convolutional sparse coding.'
date: 2014-06-17
venue: 'arXiv Preprint'
---

[Paper link](https://arxiv.org/abs/1406.4205)

<strong>Abstract:</strong><br>
In sparse coding it is common to tile an image into nonoverlapping patches, and then use a dictionary to create a sparse representation of each tile independently. In this situation, the overcompleteness of the dictionary is the number of dictionary elements divided by the patch size. In deconvolutional neural networks (DCNs), dictionaries learned on nonoverlapping tiles are replaced by a family of convolution kernels. Hence adjacent points in the feature maps (V1 layers) have receptive fields in the image that are translations of each other. The translational distance is determined by the dimensions of V1 in comparison to the dimensions of the image space. We refer to this translational distance as the stride. We implement a type of DCN using a modified Locally Competitive Algorithm (LCA) to investigate the relationship between the number of kernels, the stride, the receptive field size, and the quality of reconstruction. We find, for example, that for 16x16-pixel receptive fields, using eight kernels and a stride of 2 leads to sparse reconstructions of comparable quality as using 512 kernels and a stride of 16 (the nonoverlapping case). We also find that for a given stride and number of kernels, the patch size does not significantly affect reconstruction quality. Instead, the learned convolution kernels have a natural support radius independent of the patch size.

<strong>Recommended citation:</strong><br>
PF Schultz, DM Paiton, W Lu, GT Kenyon, "Replicating kernels with a short stride allows sparse reconstructions with fewer independent kernels", <i>arXiv Preprint</i>, 2014, arxiv: 1406.4205.

```
@misc{schultz2014replicating,
  title={Replicating Kernels with a Short Stride Allows Sparse Reconstructions with Fewer Independent Kernels},
  author={Peter F. Schultz and Dylan M. Paiton and Wei Lu and Garrett T. Kenyon},
  year={2014},
  eprint={1406.4205},
  archivePrefix={arXiv},
  primaryClass={q-bio.QM}
}
```
