---
title: 'Selectivity and robustness of sparse coding networks'
date: 2021-05-24
permalink: /posts/2021/05/selectivity-robustness/
tags:
  - response geometry
  - adversarial robustness
  - orientation selectivity
  - sparse coding
---

Selectivity and Robustness of Sparse Coding Networks
======

Dylan M Paiton
------

{: style="text-align: left"}
[Paper details](/publication/2020-11-02-selectivity-and)

### Why are real neurons recurrent?
Typical DNN architectures utilize pointwise nonlinear neurons, where a linear operation is followed
by a pointwise nonlinearity such as a rectifier (ReLU) or a sigmoid function.
These neurons are then assembled into consecutive layers that are jointly trained to minimize some
desired goal or objective.
Individual units in such networks resemble the linear-nonlinear neuron model from computational neuroscience.
In such a model, inputs are multiplied by synaptic weights, summed together in the cell body,
and the output is thresholded to give us an "activity".
This activity is analogous to an average firing rate of a neuron for a given stimulus.

![slide-1]
{: style="text-align: center"}

**Fig. 1** Deep neural networks are composed of pointwise-nonlinear neurons. On the left is a schematic
diagram of such a neuron, where the input stimulus, S, is multiplied by the neuron weights, $$\Phi$$,
summed in the cell body, and finally passed through a nonlinearity to give an activity, a. On the right
is a large number of these neurons assembled into a fully-connected neural network.
{: style="text-align: center"}

However, in biological systems lateral connectivity is found nearly everywhere. In the retina,
horizontal cells provide inhibitory feedback onto photoreceptors, which is thought to aid in
compression by reducing redundancy in the signals sent down the optic nerve.
In the LGN, inhibitory interneurons are thought to mediate spatial and temporal sharpening of image contrast.
In the primary visual cortex, inhibitory lateral connections have been proposed as a mechanism
responsible for nonlinear response properties such as end-stopping and contrast-invariant tuning.
For this work, we focused on the lateral connections in the primary visual cortex, or V1.

![slide-2]
{: style="text-align: center"}

**Fig. 2** On the left is an example of two neurons that are connected to eachother <id>laterally</it>.
On the right is a brain schematic that illustrates the visual information pathway that we will be focusing on.
{: style="text-align: center"}

V1 has a layered structure, where different depths have different types of neurons that respond
differently to visual stimuli.
Lateral and feedback connectivity is ubiquitous in these neural circuits.

![slide-3]
{: style="text-align: center"}

**Fig. 3** On the left a cross-section of the primary visual cortex in brains of different animals.
The neuron cell bodies can be seen as dark spots, while the wiring among them fills the rest of the space.
Different animals have different numbers of layers, but all animals have some sort of layered structure.
While the pictures look quite similar from layer to layer, it is hypothesized that very different computations
are performed in each layer.
{: style="text-align: center"}

For this work I will be addressing the question of "how do population interactions resulting from
lateral connections influence the information processing capabilities of model neurons?"
To better understand what I mean, lets look at this figure from Yulin Shi's lab at UC Irvine.

![slide-4]
{: style="text-align: center"}

**Fig. 4**
On the right is a figure from a paper by Xiangmin Xu et al., but modified by me to add the black squares.
Notice how the layers are highly interconnected, with recurrent loops occuring among all layers.
The black squares outline the specific connection type that I will be discussing in this post.
To make the figure, they conducted a quantitative assessment of excitatory and inhibitory connections
to excitatory neurons in mouse V1. The thickness of the line indicates the strength of the connection.
For you aficionados out there, Xu et al. did functional mapping assays using a combination two
techniques. First, slice imaging with ultraviolet stimuli to cause fluorescence and second,
morphological examination using DAPI staining, which binds strongly to adenine–thymine rich
regions in DNA. From this they were able to assess connection strengths for a large number of
excitatory and inhibitory neurons.
{: style="text-align: center"}

I really like this figure because it is easy see that the layers are highly interconnected. Lateral
inputs can be both inhibitory and excitatory, and they construct feedback loops at many levels.
For this post, I will be focusing specifically on the loops outlined in black, which are facilitated
by lateral interactions within a layer.

### Artificial neural networks
To understand the effect of lateral connectivity, I’m going to consider two different classes of
nonlinearities in neural networks:

<strong>pointwise</strong> - the output is a function of the input signal and the given neuron’s weight vector

<strong>population</strong> - the output is a function of the input signal and all of the other neuron’s weight vectors

First, let's look at pointwise nonlinear neurons.

![slide-5]
{: style="text-align: center"}

**Fig. 5**
On the left is a schematic diagram of a neuron model, just like the one in Fig. 1. On the right are
some schematic examples of what the weights look like when the network is trained to represent
natural images. These weights are well modeled using a [Gabor function](https://en.wikipedia.org/wiki/Gabor_filter).
{: style="text-align: center"}

Figure 5 has a network version of the diagram I showed before: we have inputs S, weights $$\Phi$$,
and the linear combination produces a membrane potential U. This is passed through a pointwise
nonlinearity to produce the neuron output, A.

There are a variety of ways to implement population nonlinear neural network. I have chosen a
specific method that explicitly models lateral connectivity between neurons. Here's a schematic:

![slide-6]
{: style="text-align: center"}

**Fig. 6**
The network on the right is a recurrent network that includes latral connectivity. Notice how the base
architecture is qutie similar to that on the left, with only the addition of lateral connections, G.
In order to utilize the lateral connections, the network processes inputs iteratively through time,
where the output of the neurons at one time step is fed in as input in the next. This network was
originally published by Rozell et al. in 2008, and it solves a family of
<it>Locally Competitive Altorithms</it> (LCAs).
{: style="text-align: center"}

This network adds the lateral connectivity matrix, G, to the pointwise-nonlinear version. G is assigned
to be the [Gramian matrix](https://en.wikipedia.org/wiki/Gram_matrix) of the feedforward weights
minus the [identity matrix](https://en.wikipedia.org/wiki/Identity_matrix). The result of this assignment
is that neurons inhibit each other proportionately to the overlap in their feedforward receptive fields.
For some this might be easier to understand if we write down the full equations for how the neurons
produce their outputs:

![slide-7]
{: style="text-align: center"}

**Fig. 7**
Equations for determining the outputs of each neural network type.
{: style="text-align: center"}

On the left we have the equation for the feedforward model. The membrane potential, u, is computed
by an inner-product of the feedforward weights, $$\Phi$$, with the stimulus, s. This potential is converted
to an output activation using a pointwise nonlinear function, f. The lambda subscript on f is some parameter
that changes how f behaves. The subscript k indicates which neuron we are looking at.
Next, the population nonlienarity is described via a differential equation, which governs how the
neurons change over time. This equation is derived from the objective function used for sparse coding.
The network itself is called the Locally Competitive Algorithm, or LCA, and was introduced by Chris
Rozell and colleagues in 2008. The LCA network can be implemented in standard deep learning frameworks.
There are a few established ways to do this, the least efficient of which is to “unroll” the
inference process into multiple identical, sequential layers. However, the LCA network can also be
implemented with an analog electrical circuit. Thus, although an LCA layer requires more computation
than a single feedforward layer in simulation on standard compute hardware, it is actually much more
efficient in terms of required resources than an equivalently deep feedforward network if we were
to implement it in a biological or custom hardware circuit. This efficiency comes from the fact that
you recirculate activity through the same connections and neurons, something we see nearly every
place we look in the brain. Finally, some of you might be interested to know that this LCA network
is a continuous-valued Hopfield network, as described by John Hopfield in 1984. This means that any
intuitions you might have about associative memories can be translated to this system.

The goal of this comparison is to understand how explicitly enforcing competition through a
recurrent circuit influences the possible nonlinear functions that can be implemented. Going forward
I’m going to argue that this type of circuit is not only more efficient in terms of neural resources,
it also enables outputs that are a higher-order nonlinear function of the inputs, which manifests
in increased selectivity and robustness.

### Neuron response geometry
To understand the difference between these two circuits we are going to measure the response surface
geometry of individual (artificial) neurons. We will define the <it>target neuron</it> as the neuron
that we are currently interested in studying, and we will often indiciate it mathematically with the
index k. In order to do measure the target neuron's response surface, we first need to construct a
stimulus set. Our stimulus set must be composed of images that are interesting to the neurons that
we are studying. We also want the images to be related to each other in a simple way, so that we can
easily describe changes in the target neuron's response as we go from one image to another.

Individual input images can be thought of as points in a high dimensional space. For example, a
10-by-10 image thumbnail has 10\*10=100 pixels. We can equivalently think of the image as a point
in a 100-dimensional space. Each dimension can be represented as an axis, where the position along the
axis is the value of the pixel. Our stimulus set will be images that are
[coplanar](https://en.wikipedia.org/wiki/Coplanarity), which is to say that they all lie in the same
two-dimensional plane that is sliced out of our high-dimensional input space. The next figure has an
illustration of a 3-D image space with such a 2-D slice drawn in light grey. Each point on the grey
plane can be visualized as a 3-pixel image.

![slide-8]
{: style="text-align: center"}

**Fig. 8**
Images are high-dimensional points. Cross-sections can be taken in the high-dimensional space to define
a subset of images that are coplanar. In this illustration, each yellow axis represents the value of
a pixel for a three-pixel image. The grey sheet represents a plane, or cross-section. The purple
and blue arrows are vectors that define the plane. The red arrow points in the remaining direction,
which is orthogonal to the plane.
{: style="text-align: center"}

We will choose cross sections of our input space using a specific methodology to ensure that images
in this cross section have relevance to the neuron we are studying. We then compile the images that
lie on the plane, feed them through the network, and look for connected regions where the output of
the response is equal, which is called an iso-response surface.

Since we are dealing with a simulated neuron, we will need to discretize the input space. We do this
by constructing a 2-D grid on top of a given plane. The next figure illustrates how each point in
the grid can be reshaped to be visualized as an image.

![slide-9]
{: style="text-align: center"}

**Fig. 9**
We discretize the plane into a two-dimensional grid, and collect the points to define a dataset
of images. The black arrows indicate orthogonal vectors that can be used to define the plane.
{: style="text-align: center"}

The two black arrows are orthogonal image vectors, which define our plane. Any other point in the
plane is going to look like a linear superposition of these two vectors. To see the iso-response
surfaces, we will compute the neuron’s activity for all of the images, and then recolor the points
according to the activity. This can elucidate properties and differences between pointwise and
population nonlinearities. As I said before, we want to pick a plane that is relevant for our neuron.
To do this we define one of the two plane axes as the neuron's weight vector. For now lets just assume
that the other axis is any random orthogonal vector, but we will change that later.

![slide-10]
{: style="text-align: center"}

**Fig. 10**
The horizontal axis for an image plane is always defined to be the target neuron's feedforward
weight vector.
{: style="text-align: center"}

We can now color the points according to the neuron’s output, $$a_{k}$$, and bin the values to
see the iso-response contours, which are the bin boundaries. Here's what it would look like for a
linear neuron:

![slide-11]
{: style="text-align: center"}

**Fig. 11**
The iso-response contours for a linear neuron are straight & orthogonal to the neuron's feedforward
weight vector.
{: style="text-align: center"}

Notice how the lines are straight & orthogonal to $$\Phi_{k}$$. This can be explained using introductory
linear algebra, which tells us that the linear projection operation used in the forward function does
not change its output for orthogonal perturbations. We can do this experiment again with a pointwise
nonlinear neuron:

![slide-12]
{: style="text-align: center"}

**Fig. 12**
The iso-response contours for a pointwise nonlinear neuron are <it>still</it> straight & orthogonal
to the neuron's feedforward weight vector. The nonlinearity can only change the spacing of the lines.
{: style="text-align: center"}

And we see that the iso-response lines are still straight. This is because the nonlinearity is
applied after a linear dot-product, and so the function $$f(\cdot)$$ can only change the spacing
between the lines. Next, we can look at our population network that uses recurrence to compute it's
output. The individual neurons have the same output nonlinearity, shown schematically in the figure
below. But they also receive input from other neurons, and thus as the network evolves over time
the response becomes a function of all of the neurons in the layer and hense is a population nonlinear
output.

![slide-13]
{: style="text-align: center"}

**Fig. 13**
We compare the pointwise nonlinearity against a population nonlinear network on the right. The
schematic diagram illustrates the rectification function, $$f(\cdot)$$, that was used for both
network types.
{: style="text-align: center"}

This population nonlinear setup allows the neurons to perform more complicated (i.e. less linear)
operations on the input, which results in a <it>bending</it> of the iso-response contours. The
bending comes directly from the lateral interaction term in the update equation.

![slide-14]
{: style="text-align: center"}

**Fig. 14**
The iso-response contours for a population nonlinear neuron can bend, which indicates a higher
degree of nonlinearity. The $$C$$ variable indicates the measured curvature amount.
{: style="text-align: center"}

Finally, we can repeat this process for many planes. Again, we always fix the horizontal axis to be
the target neuron's feedforward weights. But we can vary the vertical axis however we please, and for
each variation look at the curvature. Another way to think about it is we are taking samples of the
high-dimensional iso-response surface using 2-D snapshots with a single fixed axis.
Here are some examples:

![slide-15]
{: style="text-align: center"}

**Fig. 15**
The amount of curvature varies from neuron to neuron. This plot shows a variety of different neurons
in a LCA network (rows) and different chosen planes per neuron (columns). Each plane will result in
different curvature for a given target neuron, and similarly each neuron will have different curvature
for a given plane.
{: style="text-align: center"}

Notice from Figure 15 that the iso-response contours are always
bent away from the origin. This is called <it>exo-origin curavture</it>, and indicates selectivity,
which I'll explain later. First, if you're interested, you can look at the math to get a better idea
for what is going on here:

![slide-16]
{: style="text-align: center"}

**Fig. 16**
Orthogonal perturbations for linear neurons and pointwise nonlinear neurons result in no change in
the neuron's output. However, because the population nonlienar neuron's output is a function of all
other neurons, the peurturbation would have to be orthogonal to all other neuron weights to result
in zero change. For an overcomplete sparse coding network, this is highly unlikely.
{: style="text-align: center"}

In the above equations, $$p_{k}^{\top}$$ is a one-hot selection vector, to give us the response of
a single neuron, $$k$$; $$g(\cdot)$$ is the nonlinearity, either pointwise or population; and $e$ is
the perturbation. The trick is to notice that the linear operation in the pointwise nonlinear neuron,
$$Phi_{k}^{\top}(s+e)$$ is associative, and so we can isolate the change in activity to being
determined by the inner product of the perturbation onto the feedforward weights. Thus, no matter
the pointwise nonlinearity, we will always see straight contours. However, with the population
nonlinear network the output is a function of all neurons, as indicated by the full matrix $$\Phi$$.
This means that we can no longer simply describe the contours of the neurons because the graidnet
of the nonlinearity is now a function of (an arbitrary linear transofrmation of) $$s$$. Thus,
for individual neurons, the contours can be straight or bent. This contour bending results in
more interesting behavior, including improving selectivity to preferred stimulus properties as well
as adversarial robustness. Lets look at selectivity first.

### Exo-origin bent contours indicate improved orientation selectivity

To make explaining easier, I’ll sometimes swap out the actual plots for schematics:

![slide-17]
{: style="text-align: center"}

**Fig. 17**
Schematic version of iso-response contours.
{: style="text-align: center"}

Again lets consider the pointwise nonlinear neuron. What happens to the output if we take a fixed
point on the $$\Phi$$ axis and then our input along the orthogonal $$\nu$$ direction? Nothing.
As expected, the response is flat. However, for our population nonlinear network, the exo-origin
curvature results in a decrease in output, $$a_{k}$$, as we increase our perturbation in the $$\nu$$
direction. We call this <it>response attenuation</it>. This means the neuron is selective against
orthogonal perturbations. In other words, our target neuron’s output contains more information
about how close the stimulus is to its weight vector. If that is not immediately intuitive to you,
take a second to let it sink in. This is important if we consider what the “weight vector” means in
terms of real neurons. The weight vectors here correspond to the neuron’s receptive field, which
neuroscientists often use to label or identify neurons. For pointwise nonlinear neurons, we can
perturb images along any direction that is orthogonal to the neuron’s preferred stimulus and the
response will not change. However, for the LCA neuron, orthogonal perturbations result in response
attenuation. Therefore, the neuron outputs have a higher degree of correspondence to their preferred
stimulus label.

Indeed, this tells us that there are actually two types of curves that we can consider when looking
at the target neuron's response surface. They are the iso-response curvature and the response
attenuation curvature, as we plot here:

![slide-18]
{: style="text-align: center"}

**Fig. 18**
Three-dimensional plot of the iso-response contours shown in the upper left. Where before we used
color to indicate the target neuron's activation, now we are using the vertical plot axis. Now
we can clearly see the two types of curvature.
{: style="text-align: center"}

Both curvature types are important -- one measures the neuron’s attenuation when presented with
orthogonal stimulus while the other measures the iso-response surface.

![slide-20]
{: style="text-align: center"}

**Fig. 20**
Iso-response curvature tells us about adversarial robustness, while response attenuation curvature
tells us about selectivity.
{: style="text-align: center"}

Both curvature types are flat for a pointwise nonlinear neuron, but for a population nonlinear
neuron they can be different. One way to see this is to measure each curvature type for a large
number of planes. In the next figure, each subplot is a histogram of the measured iso-response and
response attenuation curvatures. We measured the curvature in 100 randomly selected neurons and 600
orthogonal planes per neuron. For all planes the horizontal axis is always the target neuron’s
weight vector, $$phi_{k}$$. We then repeated this process for three neural networks that had
different levels of [overcompleteness](https://en.wikipedia.org/wiki/Overcompleteness), which is
the ratio of neurons to pixels. In total, we presented 162 million images to the networks to assess
the response curvature. For a given plane, the curvature is the second order coefficient in a
polynomial fit (i.e. $$b$$ in $$y=ax^{2}+bx+c$$) of the iso-response contour. Thus, it is a
global second-order estimate of the iso-response curvature. In the top row of the figure, the
vertical axis was chosen in a way to increase the likelihood of competition between neurons. In
the bottom row, the vertical axis was chosen randomly.


![slide-21]
{: style="text-align: center"}
![slide-22]
{: style="text-align: center"}

**Fig. 21**
The neuron's high-dimensional curvature is almsot exclusively exo-origin. The line shading tells us
that more overcomplete networks have more curvature. Comparing the top row against the bottom row
tells us that increased competition between neurons causes more curvature.
{: style="text-align: center"}

From the above figure we can conclude that LCA neurons have a high degree of curvature in data-relevant
image planes. This is the first large-scale non-parametric estimate of curvature of sparse coding
networks. To support the theoretical argument I made before about selectivity, we conducted measured
how selective individual neurons were to oriented gratings as well as natural stimuli in a similar
experimental setup to what is used to understand selectivity of biological neurons. First, we
present individual neurons with oriented gratings that look like this:

![slide-23]
{: style="text-align: center"}

**Fig. 22**
Oriented gratings are often used to measure orientation selectivity in biological neurons.
{: style="text-align: center"}

And we found that LCA neurons are largely more orientation selective than linear
(linear autoencoder) or pointwise nonlinear (sparse autoencoder) neurons.

![slide-24]
{: style="text-align: center"}

**Fig. 23**
LCA neurons are more selective to oriented gratings than linear or sigmoid nonlinear neurons. On
the left is a histogram of the circular variance, a measure of orientation selectivity, for all of
the neurons in each model. In the middle, we show a random sampling of weights learned by each
model. On the right, we show corresponding orientation response curves. For the response curves,
the horizontal axis represents the angle of the input stimulus, which varies from $$0$$ to $$\pi$$.
The vertical axis is the response of the neuron and has been normalized by dividing the response by
the maximum across the 36 neurons shown. All networks have the same number of neurons.
{: style="text-align: center"}

Others have measured this type of selectivity for Monkey V1 neurons with similar stimuli and found
cells that exhibit the whole range of selectivity values.

![slide-25]
{: style="text-align: center"}

**Fig. 24**
Circular variance of monkey neurons.
Figure from [Ringach et al.](https://www.jneurosci.org/content/22/13/5639)
{: style="text-align: center"}


Although this histogram doesn’t look any of the histograms in Figure 23, it is important to note
that the high levels of selectivity we see here were not achievable by the linear or pointwise
nonlinear systems with oriented receptive fields. We also measured selectivity to natural stimuli
in addition to oriented gratings. Using oriented grating tuning as the sole measure of selectivity
is problematic because the stimulus set is too heavily constrained. When we only use oriented
gratings then we can trick ourselves into thinking that, for example, a highly elongated Gabor is
sufficient to produce narrow tuning. Just because a filter or receptive field looks oriented doesn’t
mean that it will signal the presence of oriented structure. A linear filter, even with a threshold
applied, is a very weak mechanism. If you probed it with a more generic class of stimuli you would
find that it responds to all sorts of stuff that’s not really even oriented. This is because its
iso-response contours are straight. Here’s an illustration of the point:

![slide-26]
{: style="text-align: center"}

**fig. 25**
two stimuli that produce equal response from a linear Gabor filter. the left is not oriented, but
the right is.
{: style="text-align: center"}

On the left we have a bright pixel in the excitatory region, and a dark pixel in the inhibitory
region. If the contrast is high enough then it will produce a strong response. Yet the image pattern
has no clearly defined orientation, and certainly it’s not what we would call “vertical." On the
other hand, if you take the same amount of pixel energy and distribute it among eight pixels, as
shown on the right, to form a clearly oriented pattern, you will get the same response from the
linear Gabor filter. So we have an elongated Gabor that should produce "narrow orientation
selectivity” in the traditional sense, yet it can’t distinguish between nonsense and a truly
oriented pattern. Due to the competition from other neurons, an LCA neuron will not be as easily
fooled by a nonsense patterns. Other neurons will also see some degree of match, especially those
with more obliquely oriented receptive fields, thus attenuate our target neuron. Whereas the actual
oriented stimulus pattern (on the right in Figure 25) will match our target neuron’s receptive field
more than any other in the population and so it will give a strong response. We demonstrate this
increased selectivity by looking at responses to natural stimuli.

![slide-27]
{: style="text-align: center"}

**fig. 26**
(Top) LCA neurons are excited by fewer natural image patches than linear neurons with identical
feedforward weights. Additionally, increasing overcompleteness reduces the average number of
selected images per neuron. (Bottom) Selected images for LCA neurons have a closer angle to their
feedforward weights than linear neurons.
{: style="text-align: center"}

In the experiment for figure 26, we computed the activation of an LCA neuron for 100,000 natural
stimuli. We then filtered for all of the stimuli that achieved at least 50% of the maximum activation
of the neuron. These are considered <it>interesting images</it>. Next we compute the angle between
these highly-activating stimuli and the neuron’s receptive field, where a lower angle means the
stimuli is more similar to the receptive field. Finally, we repeat this process for a linear neuron
with the exact same receptive field. The top plots show that the average number of
interesting images decreases as one increases LCA overcompleteness, but it remains constant for the
linear model. This means that increasing overcompleteness causes LCA neurons to become more selective,
which matches our intuition from figure 21. The bottom histograms show that the LCA neurons prefer
images that are a closer match to their feedforward weights than the linear neurons. The horizontal
axis of the histogram indicates the angle between the receptive field and the image. The vertical axis
indicates the number of stimuli that fall into a given angle bin. We thus conclude that LCA neurons
are more selective in natural environments, even though the weights are exactly the same. This simple
example demonstrates that true selectivity is a non-linear process, which can be achieved by
competition among neurons. And furthermore, using iso-response contour analysis gives you a better
indication of selectivity than probing with grating stimuli. Next we will argue that the same
principles should apply to any stimulus perturbations, including those that are derived to maximally
change the neuron's output.

### Adversarial robustness of LCA neurons
![slide-28]
{: style="text-align: center"}

**fig. 27**
Adversarial examples.
{: style="text-align: center"}

In the deep learning literature, the middle column image in Figure 26 are called
<it>adversarial examples</it> and represent one of a large set of [known failures of modern
artificial neural networks](https://www.nytimes.com/2018/11/05/opinion/artificial-intelligence-machine-learning.html).
They are small (often invisible to a person) perturbations in the input space can result in totally
different labels in the output space. In this section I will show you how these adversarial attacks
relate to iso-response contours and selectivity.

Adversarial perturbations seek to maximaly change a neurons output for a minimal change in its input.
For artificial neurons this can be done algorithmically by
[following the gradient of the neuron's activation function](https://en.wikipedia.org/wiki/Gradient_descent).
Using [Euler’s method](https://en.wikipedia.org/wiki/Euler_method), we can see how such an attack
bears out with a simplified low-dimensional example:

![slide-29]
{: style="text-align: center"}

**fig. 28**
Following the gradients of pointwise (left) and population (right) nonlinear neurons. The little
gray arrows represent gradient directions, the colored lines indicate iso-response contours, and the
colored arrow indicates the path followed by gradient ascent.
{: style="text-align: center"}

Here we are attacking an individual neuron in a single layer network. The little gray arrows
indicate gradient directions for increasing the target neuron’s output; they would go in the
opposite direction to decrease it. The colored arrow is the adversarial attack direction: the
direction that maximally changes the neuron’s output for minimal input perturbation size. Notice
how the LCA adversarial attack moves in such a way that makes the input look more like the neuron’s
target weight vector. The strength of the inner product onto $$\Phi_{k}$$ grows faster in the LCA
example, which is a direct consequence of the bent iso-response contour. Of course, gradient based
adversarial attacks ultimately perturb the input pixels in an effort to modify outputs of a
classifier, which is usually several layers later. However, the direction of perturbation is the
result of applying the [chain rule of calculus](https://en.wikipedia.org/wiki/Chain_rule), which
requires calculating input changes to perturb outputs of neurons at each layer in the network. We
argue that the curved iso-response contours cause the adversarial attack to be more constrained
such that the attacks will have to be larger in magnitude to achieve similar adversarial confidence
and they will point in directions that are more in line with the network’s preferred input
directions.

To scale up the previous experiment, we swapped out the first layer in a standard classifier with
an LCA layer. We then trained them on the MNIST dataset and looked at trajectories of adversarial
attacks. We tested variations of the dataset, the number of neurons in the first ($$a$$) layer, and
the depth of the classifier. Here are schematics for the two neteworks:

![slide-30]
{: style="text-align: center"}

**fig. 29**
On the left we show a standard feedforward network for classification. On the right we show the same
network, but where we swapped out the first layer with an LCA layer. We changed the number of neurons
in the first layer for both network types, and we also changed the classifier to be one or two layers.
{: style="text-align: center"}

We performed an unbounded, confidence based atack. This means that we iteratively perturbed the input
image until the classifier is 90% confident in the adversarial label. At this time we stop the
attack and measure the [mean squared distance, (MSD)](https://en.wikipedia.org/wiki/Mean_squared_error)
between the original image and the perturbed image. Here's the result:

![slide-31]
{: style="text-align: center"}

**fig. 30**
LCA protects against traditional adversarial attacks. The horizontal axis indicates different
experiments, and the vertical axis indicates the perturbation size required to convince the classifier
in the adversarial label.
{: style="text-align: center"}

In addition to requiring a larger MSD, LCA qualitatively influences the perturbations. We can see
that the perturbation clearly moves towards the target class.

We can test the predictions of our smaller-scale experiment in figure 28 by projexcting one of the
attack trajectories into an image plane that was spanned by two LCA weight vectors chosen to have
semantic similarity to the original image label and the adversarial target label. Here's what that
looked like:

![slide-32]
{: style="text-align: center"}

**fig. 31**
LCA protects against traditional adversarial attacks. The horizontal axis indicates different
experiments, and the vertical axis indicates the perturbation size required to convince the classifier
in the adversarial label. The neuron’s weight vectors are displayed as images along with the input
image, a 1, and the final attack output, which resembles the final classification output, a 3. PGD
stands for "Projected Gradient Descent", which is the type of adversarial attack that was used.
{: style="text-align: center"}

We found that the predictions made with the small-scale model appear to play out on the larger scale.
Notice that the final adversarial image looks more like the “3” basis vector after the attack. We
also tested the network on CIFAR:

![slide-33]
{: style="text-align: center"}

**fig. 32**
LCA adversarial experiments on the CIFAR natural images dataset.
{: style="text-align: center"}

LCA seems to do better overall on CIFAR, but the effect is much less convincing. Looking at the
perturbations themselves, you can see a clear impact but the complexity of the data makes the
category relevance unclear. Our hypothesis is that the strength of the effect will be determined by
the ability of the LCA neurons to effectively span the high-density regions of the data distribution
and the extent to which the actual target of the attack relies on the activation of any given
neuron or set of neurons being maximized. In other words, more overcompleteness should help and a
single LCA layer will not be as effective when paired with deeper classifiers or more complicated
datasets. We believe this begs for the development of deeper generative architectures with
population non-linearities.


To conclude, I want to ackowledge my co-authors, without whom this work never would have been
possible. I also thank the funding sources that allowed for this work to be computed:

![slide-34]
{: style="text-align: center"}


[slide-1]: /images/selectivity-robustness/slide-1.png
[slide-2]: /images/selectivity-robustness/slide-2.png
[slide-3]: /images/selectivity-robustness/slide-3.png
[slide-4]: /images/selectivity-robustness/slide-4.png
[slide-5]: /images/selectivity-robustness/slide-5.png
[slide-6]: /images/selectivity-robustness/slide-6.png
[slide-7]: /images/selectivity-robustness/slide-7.png
[slide-8]: /images/selectivity-robustness/slide-8.png
[slide-9]: /images/selectivity-robustness/slide-9.png
[slide-10]: /images/selectivity-robustness/slide-10.png
[slide-11]: /images/selectivity-robustness/slide-11.png
[slide-12]: /images/selectivity-robustness/slide-12.png
[slide-13]: /images/selectivity-robustness/slide-13.png
[slide-14]: /images/selectivity-robustness/slide-14.png
[slide-15]: /images/selectivity-robustness/slide-15.png
[slide-16]: /images/selectivity-robustness/slide-16.png
[slide-17]: /images/selectivity-robustness/slide-17.png
[slide-18]: /images/selectivity-robustness/slide-18.png
[slide-19]: /images/selectivity-robustness/slide-19.png
[slide-20]: /images/selectivity-robustness/slide-20.png
[slide-21]: /images/selectivity-robustness/slide-21.png
[slide-22]: /images/selectivity-robustness/slide-22.png
[slide-23]: /images/selectivity-robustness/slide-23.png
[slide-24]: /images/selectivity-robustness/slide-24.png
[slide-25]: /images/selectivity-robustness/slide-25.png
[slide-26]: /images/selectivity-robustness/slide-26.png
[slide-27]: /images/selectivity-robustness/slide-27.png
[slide-28]: /images/selectivity-robustness/slide-28.png
[slide-29]: /images/selectivity-robustness/slide-29.png
[slide-30]: /images/selectivity-robustness/slide-30.png
[slide-31]: /images/selectivity-robustness/slide-31.png
[slide-32]: /images/selectivity-robustness/slide-32.png
[slide-33]: /images/selectivity-robustness/slide-33.png
[slide-34]: /images/selectivity-robustness/slide-34.png
