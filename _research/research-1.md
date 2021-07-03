---
title: "Neuron Response Geometry"
excerpt: "Understanding neural computation using differential geometry<br/>
<img src='/images/contours_and_gradients.png'
    width=800 />"
collection: portfolio
---

Individual neurons in the brain perform computations on their inputs to produce outputs.
We first learned this in the late 1950s, when Nobel prize winners [Hubel and Wiesel](https://www.brains-explained.com/how-hubel-and-wiesel-revolutionized-neuroscience/) discovered cat neurons that respond selectively to moving edges.
My research looks at how the neuron response changes as one smoothly modulates the input signal.
When we do this, the neuron response maps out a surface that has curvature.
This curvature can help us describe properties about the computation that the neuron is performing.
We can predict neural robustness properties, group similar neurons together, and define what in the world neurons are selective for or invariant to.

This research area is also highly applicable to artificial neurons in deep neural networks.
For example, it has been used by myself and [others](https://openaccess.thecvf.com/content_CVPR_2019/html/Moosavi-Dezfooli_Robustness_via_Curvature_Regularization_and_Vice_Versa_CVPR_2019_paper) to explain failure modes such as susceptibility to [adversarial attacks](https://www.nytimes.com/2018/11/05/opinion/artificial-intelligence-machine-learning.html).

My primary PhD thesis topic was on this method.
My work allowed us to gain a better understanding of selectivity and adversarial robustness properties of the network.
To learn more, you can check out the [tweet](https://twitter.com/DylanPaiton/status/1333480509893206018?s=20) for a super short description, [my blogpost]({{site.url}}/posts/2021/05/response-geometry/) high-level overview, or read [the paper](/publication/2020-11-02-selectivity-and) for the details.
