---
title: 'A deconvolutional competitive algorithm for building sparse hierarchical representations'
collection: publications
permalink: /publication/2016-05-24-a-deconvolutional
excerpt: 'A heirarchical sparse coding network that learns bandpass decompositions of natural images.'
date: 2016-05-24
venue: 'Proceedings of the 9th EAI International Conference on Bio-inspired Information and Communications Technologies'
---

[Paper link](https://dl.acm.org/doi/abs/10.4108/eai.3-12-2015.2262428)

<strong>Abstract:</strong><br>
Sparse coding methods have been used to study how hierarchically organized representations in the visual cortex can be learned from unlabeled natural images. Here, we describe a novel Deconvolutional Competitive Algorithm (DCA), which explicitly learns non-redundant hierarchical representations by enabling competition both within and between sparse coding layers. All layers in a DCA are trained simultaneously and all layers contribute to a single image reconstruction. Because the entire hierarchy in a DCA comprises a single dictionary, there is no need for dimensionality reduction between layers, such as MAX pooling. We show that a 3-layer DCA trained on short video clips exhibits a clear segregation of image content, with features in the top layer reconstructing large-scale structures while features in the middle and bottom layers reconstruct progressively finer details. Compared to lower levels, the representations at higher levels are more invariant to the small image transformations between consecutive video frames recorded from hand-held cameras. The representation at all three hierarchical levels combine synergistically in a whole image classification task. Consistent with psychophysical studies and electrophysiological experiments, broad, low-spatial resolution image content was generated first, primarily based on sparse representations in the highest layer, with fine spatial details being filled in later, based on representations from lower hierarchical levels.

<strong>Recommended citation:</strong><br>
DM Paiton, SY Lundquist, WB Shainin, X Zhang, PF Schultz, and GT Kenyon. "A Deconvolutional Competitive Algorithm for Building Sparse Hierarchical Representations." <i>Proceedings of the 9th EAI International Conference on Bio-inspired Information and Communications Technologies (formerly BIONETICS) (BICT'15).</i> ICST (Institute for Computer Sciences, Social-Informatics and Telecommunications Engineering), Brussels, BEL, 535–542. DOI:https://doi.org/10.4108/eai.3-12-2015.2262428

```
@inproceedings{paiton2016deconvolutional,
  author={Paiton, Dylan M and Lundquist, Sheng Y and Shainin, William B and Zhang, Xinhua and Schultz, Peter F and Kenyon, Garrett T},
  title={A Deconvolutional Competitive Algorithm for Building Sparse Hierarchical Representations},
  year={2016},
  isbn={9781631901003},
  publisher={ICST (Institute for Computer Sciences, Social-Informatics and Telecommunications Engineering)},
  address={Brussels, BEL},
  url={https://doi.org/10.4108/eai.3-12-2015.2262428},
  doi={10.4108/eai.3-12-2015.2262428},
  booktitle={Proceedings of the 9th EAI International Conference on Bio-Inspired Information and Communications Technologies (Formerly BIONETICS)},
  pages={535–542},
  numpages={8},
  keywords={hierarchical model, sparse coding, deconvolution, visual cortex, convolutional neural network, deep learning},
  location={New York City, United States},
  series={BICT'15}
}
```
