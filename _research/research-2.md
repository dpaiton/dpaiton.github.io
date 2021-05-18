---
title: "Hierarchical models of natural scenes"
excerpt: "Proposing candidate models for biological scene representation<br/>
<img src='/images/subspace_lca.png'
    width=400 />"
collection: portfolio
---

A long-standing interest of mine is to develop hierarchical probablistic models of natural scenes.
While representing real-world scenes in terms of true probabilities is a mathematician's
nightmare, approximating such a representation has proven to be an extremely effective way to
model scenes in an efficient and robust way.
Perhaps just as importantly, so far it has shown a high degree of correspondence with what we see
in real brains. Most of my works so far have built on the Sparse Coding framework,
which represents a leading candidate for computation in the early vision system of mammals.

<strong>Nonlinear disentanglement with variational probabilistic neural networks</strong><br>
Most recently, I lead a team to conduct a research project on understanding disentanglement in
neural networks. The goal of disentanglment research is to learn an encoding function that can map
inputs, such as images or videos, to the underlying factors that varied to produce those images,
such as an object's position, identity, or orientation. We used neural networks that approximate
probabilistic inference to encode the images. You can read more about it in
[the paper](https://openreview.net/forum?id=EbIDjBynYJ8).

<strong>A nonlinear, hierarchical subspace sparse coding network</strong><br>
I also recently published an alternative hierarchical sparse coding model that learns to represent
natural scenes in terms of subspaces.
These subspaces share properties, such as the orientation of an edge, but also allow for some
variation, such as the position of an edge.
By learning the subspaces directly from natural image data, we are able to make concrete statements
about what invariances could exist in a brain.
You can read more about it in [my blogpost]({{site.url}}/blogs/responsegeometry) or check out
[the paper](https://dl.acm.org/doi/abs/10.1145/3381755.3381765) to learn more.

<strong>The sparse manifold transform</strong><br>
The Sparse Manifold Transform is an exciting development in hierarchical modeling of natural
scenes that was developed by my colleague
[Yubei Chen](https://redwood.berkeley.edu/people/yubei-chen/).
The network is explicitly represents videos in terms of a hierarchy composable elements,
and is heavily inspired by recent ideas in neuroscience about how the brain might represent
scene information using smooth manifolds.
I co-authored [the paper](https://papers.nips.cc/paper/2018/hash/8e19a39c36b8e5e3afd2a3b2692aea96-Abstract.html),
which gives all of the details.

<strong>Deep deconvolutional sparse coding for scene spatial frequency decomposition</strong><br>
While working in Gar Kenyon's lab, I developed a model that decomposes visual scenes into spatial
frequency bands using a hierarchical convolutional sparse coding model. You can read all about it
from [the paper](https://dl.acm.org/doi/abs/10.4108/eai.3-12-2015.2262428).
The work has been extended by Edward Kim et al. in [this paper](https://arxiv.org/abs/2011.11167),
who present a very interesting model of higher level brain processing.
