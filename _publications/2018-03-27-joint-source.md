---
title: 'Joint source-channel coding with neural networks for analog data compression and storage'
collection: publications
permalink: /publication/2018-03-27-joint-source
excerpt: 'A convolutional autoencoder with divisive normalization enables digital image storage on simulated emerging memristive devices.'
date: 2018-03-27
venue: 'Data Compression Conference'
---

[Paper link](https://ieeexplore.ieee.org/abstract/document/8416587)

<strong>Abstract:</strong><br>
We provide an encoding and decoding strategy for efficient storage of analog data onto an array of Phase-Change Memory (PCM) devices. The PCM array is treated as an analog channel, with the stochastic relationship between write voltage and read resistance for each device determining its theoretical capacity. The encoder and decoder are implemented as neural networks with parameters that are trained end-to-end to minimize distortion for a fixed number of devices. To minimize distortion, the encoder and decoder must adapt jointly to the statistics of images and the statistics of the channel. Similar to Balle et al. (2017), we find that incorporating divisive normalization in the encoder, paired with de-normalization in the decoder, improves model performance. We show that the autoencoder achieves a rate-distortion performance above that achieved by a separate JPEG source coding and binary channel coding scheme. These results demonstrate the feasibility of exploiting the full analog dynamic range of PCM or other emerging memory devices for efficient storage of analog image data.

<strong>Recommended citation:</strong><br>
R Zarcone, DM Paiton, AA Anderson, JH Engel, HSP Wong and BA Olshausen, "Joint Source-Channel Coding with Neural Networks for Analog Data Compression and Storage," <i>Data Compression Conference</i>, 2018, pp. 147-156, doi: 10.1109/DCC.2018.00023.

```
@INPROCEEDINGS{zarcone2018joint,
  author={Zarcone, Ryan and Paiton, Dylan and Anderson, Alex and Engel, Jesse and Wong, H.S. Philip and Olshausen, Bruno},
  booktitle={2018 Data Compression Conference},
  title={Joint Source-Channel Coding with Neural Networks for Analog Data Compression and Storage},
  year={2018},
  volume={},
  number={},
  pages={147-156},
  doi={10.1109/DCC.2018.00023}
}
```
