---
title: 'Towards nonlinear disentanglement in natural data with temporal sparse coding'
collection: publications
permalink: /publication/2021-05-24-towards-nonlinear
excerpt: 'We construct and analyze new datasets for evaluating disentanglement on natural videos. We also propose a temporally sparse prior for identifying the underlying factors of variation in natural videos.'
date: 2021-05-24
venue: 'International Conference on Learning Representations'
---

[Paper link](https://openreview.net/forum?id=EbIDjBynYJ8)

<strong>Abstract:</strong><br>
Disentangling the underlying generative factors from complex data has so far been limited to carefully constructed scenarios. We propose a path towards natural data by first showing that the statistics of natural data provide enough structure to enable disentanglement, both theoretically and empirically. Specifically, we provide evidence that objects in natural movies undergo transitions that are typically small in magnitude with occasional large jumps, which is characteristic of a temporally sparse distribution. To address this finding we provide a novel proof that relies on a sparse prior on temporally adjacent observations to recover the true latent variables up to permutations and sign flips, directly providing a stronger result than previous work. We show that equipping practical estimation methods with our prior often surpasses the current state-of-the-art on several established benchmark datasets without any impractical assumptions, such as knowledge of the number of changing generative factors. Furthermore, we contribute two new benchmarks, Natural Sprites and KITTI Masks, which integrate the measured natural dynamics to enable disentanglement evaluation with more realistic datasets. We leverage these benchmarks to test our theory, demonstrating improved performance. We also identify non-obvious challenges for current methods in scaling to more natural domains. Taken together our work addresses key issues in disentanglement research for moving towards more natural settings. 

<strong>Recommended citation:</strong><br>
David A. Klindt\*, Lukas Schott\*, Yash Sharma\*, Ivan Ustyuzhaninov, Wieland Brendel, Matthias Bethge\*, Dylan M Paiton\*. "Towards nonlinear disentanglement in natural data with temporal sparse coding." <i>Proceedings of the International Conference on Learning Representations</i>. 2021

```
@inproceedings{klindt2021towards,
  title={Towards Nonlinear Disentanglement in Natural Data with Temporal Sparse Coding},
  author={David A. Klindt and Lukas Schott and Yash Sharma and Ivan Ustyuzhaninov and Wieland Brendel and Matthias Bethge and Dylan M. Paiton},
  booktitle={International Conference on Learning Representations},
  year={2021},
  url={https://openreview.net/forum?id=EbIDjBynYJ8}
}
```
