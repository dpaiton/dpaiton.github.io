---
title: 'Response geometry explains adversarial robustness'
date: 2021-06-26
permalink: /posts/2021/06/adversarial-robustness/
tags:
  - response geometry
  - adversarial robustness
  - differential geometry
---

Defending against adversarial attacks is critical for modern AI applications. Here I describe how we can use an analytic method for summarizing the input-output relationship of neurons to predict how easy it will be to attack them.
------

This writeup is builds on my [previous post]({{site.url}}/posts/2021/05/response-geometry/) about using the curvature of a neuron's response surface to understand its behavior.
If that idea is unfamiliar to you, then I recommend reading that post before this one.

### Adversarial examples
Adversarial examples are a worst-case demonstration of a deep neural network's (DNNs) inability to gracefully cope with small targeted shifts in their inputs.
To construct one, an adversary must modify, or _perturb_, the input in a very small but specific way such that the output of the DNN changes significantly.
The figure below is adapted from an iconic example provided in one of the [first papers](https://arxiv.org/abs/1312.6199) demonstrating adversarial attacks on DNNs, authored by Dr. Christian Szegedy and colleagues.
The original input images are in the left column and they belong to the categories "mantis" and "dog".
The images in the middle column are adversarial perturbations, which can be added to the original inputs to produce adversarial images in the right column.
All of the adversarial images are labeled as "ostrich" by the network.
The perturbations have to be magnified to allow you to see what they look like because their original scale is at the lower bound of what a typical computer image can convey.<a href="{{page.url}}#dagger"><sup>†</sup></a>
Importantly, these attacks do not just apply to images.
They have been demonstrated to work pretty much anywhere that DNNs are useful, and they almost always seem innocuous or are imperceptible for a person.
Successful demonstrations include tricking [text analyzers](https://arxiv.org/pdf/1804.07998.pdf) that are used for product review summaries, social media feeds, and search result rankings; tricking smart home devices to [execute a command](https://arxiv.org/abs/1801.01944); and tricking a self-driving car to [mistake a stop sign for a yield sign](https://arxiv.org/abs/1707.08945).

![adversarial-images]
{: style="text-align: center; font-size:11pt"}

**fig. 1**
adversarial images
{: style="text-align: center; font-size:11pt"}

To be more precise, _adversarial examples_ are perturbations to the input of a network that are small in magnitude, but cause a big enough shift in its output to change its behavior.
While this is not a hard requirement, they are also often semantically unrelated to the new output of the network -- for example the "ostrich" perturbation is not recognizable as an ostrich.
These two criteria, small size and uninterpretable semantic quality, are what makes them so interesting to researchers.
There are adversarial perturbations that could trick a classifier but would not be interesting, such as adding a horizontal stroke to a "0" to trick a classifer into calling it an "8", or [putting wheels on a canoe](https://vikebike.com/vikebike01_FullRes.jpg) to trick the classifier into thinking it is a bicycle.
If these obvious attacks were what was demonstrated by Dr. Szegedy and colleagues then few other researchers would have paid attention.
Unfortunately, we do not have a good way to measure "semantic quality," so the research community can only use the average size of the perturbations to quantify _adversarial robustness_, which indicates how suceptible a network is to adversarial attacks.
However, they do often also show examples of perturbed inputs to allow readers to judge for themselves whether the semantic content is reasonable given the original and new network outputs.
Now that we have defined the adversarial attacks and robustness we can better understand how such attacks are conducted.

### Following gradients
There are many methods for constructing adversarial perturbations and they can roughly be categorized by how much access they have to the network they are attacking.
All of the methods, however, seek the same goal: to maximally change a function's output given a minimal change in its input.
To illustrate how attackers achieve this, let's consider an example where the function input is the position on a map and the function output is the elevation at that position.
We can summarize such a function with a [topographic map](https://openpress.usask.ca/geolmanual/chapter/overview-of-topographic-maps/).
Topographic maps use iso-elevation lines to indicate altitude, which means your elevation is constant if you walk along the line.
This is illustrated in the following figure:

![topographic-elevation]
{: style="text-align: center; font-size:11pt"}

**fig. 2**
Topographic maps indicate altitude with iso-elevation lines.
{: style="text-align: center; font-size:11pt"}

Following the goal we defined earlier, we want to determine how to minimally change our position and maximally increase our elevation.
There are two things to consider to solve this: one is where to start and the second is what direction to walk from there.
In the case of adversarial examples the starting position is fixed to be the original input, like the dog or mantis above.
In the figure below we indicate a starting position with the smiley face.
The optimal direction to climb at any given position is orthogonal to the iso-elevation lines.
This orthogonal direction aligns with the maximum gradient of the function, which is a term in math that has an intuitive mapping on to gradients in common language, such as a temperature gradient or the grade of a hill.
That is to say that the _gradient_ indicates how much the function's output changes for a given minimal directional change in the function's input.

![topographic-ascent]
{: style="text-align: center; font-size:11pt"}

**fig. 3**
A path that is orthogonal to the iso-elevation lines in a topographic map indicates the fastest way to change altitude.
{: style="text-align: center; font-size:11pt"}

All methods for finding adversarial examples are derived from this same guiding principal, which is to use _gradients_ to determine what direction to go.
I'll explain in the wrap-up that this is considered a _local_ perspective on finding an optimal direction.
In my [previous post]({{site.url}}/posts/2021/05/response-geometry/) on iso-response contours, I explained how images can be thought of as arrows reaching to points in a high-dimensional space.
Along this line of reasoning, an adversarial perturbation can be drawn as an arrow extending from the original input with some direction and magnitude.
This the same concept that we are using in figure 3: each red arrow starts at some position and ends at some other position.
And just like each point on that map has an analogous location in the world (in this case Hawaii), each point in our neuron iso-response contour figures have analogous images.
To determine the input perturbation that maximally changes a function's output, we choose arrow directions that are orthogonal to the function's iso-response contours.
Usually that function is an entire neural network, but the explanation works just as well if we instead use a single neuron.

### Neuron robustness
Scientists use a large variety of different types of models to describe neurons in both neuroscience as well as artificial intelligence research.
One way to organize the models is by their level of abstraction, which is describes the tradeoff between biological accuracy and computational complexity.
Indeed, one major goal of computational neuroscience is to discover the level of abstraction that has minimal complexity while still emulating the behavior of biological organisms.
A neuron's response surface geometry can help us quantify differences in behavior, such as a model's selectivity, invariance, or robustness.
It can also help us understand complexity, in that neurons with straight iso-response contours are minimally complex and so any deviation from straight contours will indicate less complex (i.e. less _linear_) processing.
To apply the above climbing example to explaining neuron robustness, let's consider two models: one that produces straight iso-response contours and one that produces exo-origin bent iso-response contours.
The next figure shows the iso-response contours for these models as well as the arrows indicating optimal adversarial perturbation directions for increasing their response with minimal perturbation size.

![adversarial-neuron-attack]
{: style="text-align: center; font-size:11pt"}

**fig. 4**
Neurons with different iso-response contour curvature will require in different adversarial attacks.
{: style="text-align: center; font-size:11pt"}

In the above figure there are two arrow types.
The short gray arrows indicate optimal gradient directions.
The longer colored arrows indicates an optimal adversarial perturbation from a given starting point, which is indicated by a black circle.
The numbers above the colored iso-response lines indicate neuron outputs (in arbitrary units), which generally increase to the right.
The $$\Phi_{k}$$ variable indicates the direction of the neuron's maximally exciting image, or MEI.
As the name implies, this image tells us what feature the neuron is most excited about in the world.
In biological and artificial neural networks, the MEI features vary in complexity as one ascends a hierarchy of processing stages, from simple edges at the lowest levels to faces and complex objects at the highest levels.
Both neuroscientists and deep learning researchers often use a neuron's MEI to label that neuron, for example as a "dog detector" or a "vertical edge detector."
All of this matters when we think about an adversarial attack on the neuron.

As I discussed earlier, if we modify a picture of the number "0" to look more like an "8", then we should not be too surprised if a neuron that has an MEI that looks like an "8" to get more excited.
For both neurons, as we take steps in the adversarial direction, the output will look more and more like the neuron's MEI.
However, this will happen faster for the neuron with the curved response contours, to the point where the input eventually just ends up looking exactly like the MEI.
This means that the optimal perturbation direction for neurons with curved response contours is more aligned with their MEIs than for neurons with straight contours.
Thus, if we assume that neurons are interested in semantically relevant features, then we hypothesize that the resulting adversarial perturbations are going to be more semantically relevant if the neurons with curved response contours.

When we have a network of these neurons, then it turns out that the curved contours can result in larger overall perturbations as well.
An ensemble of neurons with bent contours will constrain the adversary, limiting its perturbation options.
The next figure demonstrates the idea, where we show at all possible pixel values that will result in an increased activation for neuron $$k$$.

![constrained-optimization]
{: style="text-align: center; font-size:11pt"}

**fig. 5**
Bent iso-response contours constraint the space of possible inputs to increase a neuron's activation.
{: style="text-align: center; font-size:11pt"}

The dotted box represents the range of allowable pixel values for input images.
The yellow shaded area represents all possible inputs that would increase the activation of neuron $$k$$ if it had straight iso-response contours.
Conversely, the green shaded area represents all possible inputs that would increase the activation of neuron $$k$$ if it had bent iso-response contours.
As you can see if we overlay the two shapes, any amount of bending will necessarily reduce the number of possible pixels.
This means an adversary will have fewer options to choose from if it wishes to increase the output of neuron $$k$$.

### Wrap-up ###


### Why are real neurons recurrent?
<!--- linear, nonlinear, & recurrent models explinations  -->
As usual, I want to start with the most simple neuron model -- a _linear_ model.
This type of neuron performs a weighted sum of its inputs to produce its outputs.

<!--- recurrence can cause response-contour bending  -->


### Response geometry determines adversarial attack directions
<!---  attacks will be orthogonal to contours  -->

<!--- constraining the search space  -->

As a running example let's consider a neural network that takes images as inputs and produces labels for those images as output.
Importantly, though, the concepts explained here can be applied broadly in all AI supported industries, including self driving car technology, speech recognition, and robot training.
Adversarial robustness is currently one of the top research areas in AI, and has critical importance for almost all AI industries (which these days can almost be replaced with “almost all industries”).

Adversaries cause large and erroneous outputs for small input perturbations.
To do this, they figure out what input perturbations cause a maximal change in the final neuron’s output.
There are a few methods for determining the perturbation that we don’t need to get into here.
Normally it is a complicated process, since the input is passed through several consecutive layers of neurons before reaching the final output.
To simplify the explanation let’s consider a single neuron.
This is reasonable, since the final output is a function of many single neurons.
So if we want to maximally change a single output neuron’s response we can do that by maximally changing the response of its input neurons in the layer below it, and so forth down to the first layer.
We will call the perturbation direction that causes maximal output change for minimal input change an adversarial direction.

Let’s start with a counter-example.
What would we do if we wanted to change an input to _minimally_ change the output? As I explained in my previous post, we can change the input as much as we want and not change the output at all if we perturb along the iso-response contours.
The iso-response contours of a neuron have an important relationship to adversarial directions, which is that they are orthogonal, or perpendicular.
Hopefully this is intuitively clear if you think about it a bit, because it is true in general for “iso” lines.
For example, look at a contour map (also known as a topographic map) of a mountain range.
The contour lines in these maps are iso-elevation lines, indicating that walking along the line means your altitude will not change.
The altitude difference from line to line is also set to be equal.
This means that lines that are closer together indicate a steeper slope.
So if we want to climb quickly, we should start at a spot where the lines are closest together.
This determines our starting position.
Once we are at the starting position, how do we decide what direction to go? If you want to maximally change your elevation with minimal steps, you must move perpendicular to the iso-elevation lines.
This is true regardless of the line spacing and is easiest to think about if we imagine that all of the contour lines are equally spaced, i.e. the mountain has the same slope everywhere.
We will illustrate the idea by looking at the 2D neuron iso-response maps that we used in the previous blog post.
As a starting point, let's consider a linear neuron, which has straight and equally-spaced iso-response lines.
The horizontal axis is the neuron’s MEI, or maximally exciting image, which was determined to be the image that causes the largest output.
If we perturb an input orthogonal to the iso-response lines, then the activation maximally changes.
If we go in the direction of the MEI (to the right in this case) then the activation maximally grows, and vice versa for the opposite direction.

### Deep neural networks
In reality we consider situations where neural networks perform interesting tasks, which requires many neurons to be assembled into a layered structure.
If we expand our previous example to consider a two-layer network, as shown below, we can appreciate how the importance of iso-response contour shape.

### Curvature and adversarial defense
<!--- MNIST & CIFAR results  -->

### Wrapup
At a high level, adversarial examples are one of many pieces of evidence supporting the idea that DNNs don’t [perceive the world like we do](https://www.nytimes.com/2018/11/05/opinion/artificial-intelligence-machine-learning.html).
Given that there are other mismatches, exclusively solving adversarial succeptibility in DNNs is unlikely to align human and machine perception.
However, adversarial examples still serve as a valuable test of ideas in neural computation: if we have an idea to explain perception, then our model of the idea should be robust to adversarial attacks.

One detail that would be remiss of me to ignore is the spacing between the iso-response contours.
You might have noticed that the bent and straight contorus in figure 4 were (approximately) equally spaced.
The optimal adversarial perturbation direction at any given point is indeed orthogonal to these contours.
However, if the spacing is not guaranteed to be equal then there may be a suboptimal direction to take _at that moment_ that ultimately achieves the goal with many fewer steps.
If you wish to climb a mountain in as few steps as possible, for example, you might need to walk in a sub-optimal direction for a little while to get to a cliff face that will let you climb at the fastest rate.
This distinction is subtle, but important when we are trying to build defenses against adversarial attacks.
The difference I'm describing is between a _local_ and a _global_ optimization process.
Importantly, though, a local perspective, such as knowing that there is a cliff on the other side of some valley, is rare when attacking neural networks because of the high dimensionality of the inputs.

Thus, iso-response contour _curvature_ provides part of the story of adversarial suceptiblity, and iso-response contour _spacing_ provides another part.
No one has provided a complete picture of adversarial volnerability using iso-response geometry, and so we do not know if there are more aspects to consider.
However, hopefully this post increased your understanding and demonstrated yet another way that the geometry of a neuron's response surface can teach us about how that neuron processes its inputs.

Please see [my paper](/publication/2020-11-02-selectivity-and) for further reading on iso-response contours and adversarial robustness.

### Footnotes
<a name="dagger">†</a> Computer images are composed of pixels that each take on one of 256 possible values. This is the same as saying that they are 8-bit images. If one were to rescale the pixel values to be between 0 and 1, then the average pixel perturbation size for the examples from Szegedy et al. is 0.006. This corresponds to changing a single pixel by about 2, or in reality changing many pixels by less than 1. As such, adversarial perturbations are often smaller than the available bit-depth of current displays.

[adversarial-images]: /images/adversarial-robustness/adversarial-robustness.png
[topographic-elevation]: /images/adversarial-robustness/topographic-elevation.png
[topographic-ascent]: /images/adversarial-robustness/topographic-ascent.png
[adversarial-neuron-attack]: /images/adversarial-robustness/adversarial-neuron-attack.png
[perturbation-sizes]: /images/adversarial-robustness/perturbation-sizes.png
[constrained-optimization]: /images/adversarial-robustness/constrained-optimization.png
