---
title: "Neuron Response Geometry"
excerpt: "Understanding neural computation using differential geometry<br/>
<img src='/images/contours_and_gradients.png'
    width=800 />"
collection: portfolio
---

Individual neurons in the brain perform computations on their inputs to produce outputs. We first
learned this in the late 1950s, when Nobel prize winners
[Hubel and Wiesel](https://youtu.be/IOHayh06LJ4) discovered cat neurons that respond
selectively to moving edges. My research looks at how the neuron response changes as one smoothly
modulates the input signal. When we do this, the neuron response maps out a surface that has curvature.
This curvature can help us describe properties about the computation that the neuron is performing.
For example, we can define what in the world the neuron is selective for or invariant to. We can also
predict how robust the neuron is to noise.

This research area is also highly applicable to artificial neurons in deep neural networks.
For example, it has been used by myself and others to explain failure modes such as succeptibility to
[adversarial attacks](https://www.nytimes.com/2018/11/05/opinion/artificial-intelligence-machine-learning.html).

My primary PhD research topic was to use a method from experimental neuroscience to characterize
the curvature of neurons in an artificial neural network. My work allowed us to gain a better
understanding of selectivity and adversarial robustness properties of the network.

You can check out the [tweet](https://twitter.com/DylanPaiton/status/1333480509893206018?s=20)
for a super short description, [my blogpost]({{site.url}}/posts/2021/05/selectivity-robustness/)
high-level overview, or read [the paper](https://jov.arvojournals.org/article.aspx?articleid=2772000)
for the details.
