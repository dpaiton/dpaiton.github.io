---
title: 'Response geometry explains adversarial robustness'
date: 2021-06-24
permalink: /posts/2021/06/adversarial-robustness/
tags:
  - response geometry
  - adversarial robustness
  - differential geometry
---

Robustness and response curvature
======

Dylan M Paiton
------

This writeup is builds on my [previous post]({{site.url}}/posts/2021/05/response-geometry/) about using the curvature of a neuron's response surface to understand its behavior.
If that idea is unfamiliar to you, then I recommend reading the previous post before this one.

### Adversarial examples
Adversarial examples are a worst-case demonstration of a deep neural network's (DNNs) inability to gracefully cope with small shifts in their inputs.
To construct one, an adversary must modify, or _perturb_, the input in a very small but specific way such that the output of the DNN changes significantly.
For DNNs trained on images, these perturbations can be so small that many monitors can't display the difference in pixel values.
The figure below is adapted from an iconic example provided in [one of the early papers demonstrating adversarial attacks on DNNs, authored by Christian Szegedy and colleagues.]{https://arxiv.org/abs/1312.6199}.
The original input images are in the left column and belong to the categories "speaker", "mantis", and "dog".
The images in the middle column are adversarial perturbations, which can be added in to the original inputs to produce the adversarial inputs in the right column.
All of the adversarial input images are labeled as "ostrich" by the network.
In this example, the average perturbation size is 0.6% of one pixel, but they are magnified in the figure to demonstrate what they look like.
Importantly, these attacks do not just apply to images.
They have been demonstrated to work pretty much anywhere that DNNs are useful, and they almost always seem innocuous or are imperceptible for a person.
Successful demonstrations include skewing text analysis that is used for [product review summaries, social media feeds, and search result rankings]{https://arxiv.org/pdf/1804.07998.pdf}, tricking your smart home device to [execute a command]{https://arxiv.org/abs/1801.01944}, and tricking a self-driving car to [mistake a stop sign for a yield sign]{https://arxiv.org/abs/1707.08945}.
![adversarial-images]

At a high level (and in my humble opinion), I see adv examples as one of many pieces of evidence supporting the idea that DNNs don’t “perceive” like we do.
That’s the long and short of it.
A DNN practitioner may some day find a way to improve adv robustness to some industry-accepted level, but that is just whacking one of many moles.
So I don’t think a research agenda focused on improving DNN adv defense is wise.
However, adv examples do still serve as an interesting test of our own ideas.
If we have an idea to explain perception, then our model better be robust to adv attacks.
Also, if our goal is understanding how DNNs work, and less fixing DNNs, then there’s a lot to be gained in studying adv examples.

My favorite perspective on adv examples is that of Seyed-Mohsen Moosavi-Dezfooli.
He has several papers on the topic, which you can find on his scholar page, although tbh I’d recommend just skipping right to his PhD thesis.
It is really well written and covers all of the papers.
Part II is what you’re mostly interested in, but the whole thing is good. https://infoscience.epfl.ch/record/271933
He takes a geometric perspective by looking at the curvature of the network’s decision boundaries to assess robustness.
I believe such a geometric viewpoint is going to be necessary, and if anything we should be looking more closely at how to use differential geometry to understand the loss surface (wrt image perturbations — not to be confused with the loss surface wrt to parameter perturbations).
I’d also recommend this paper: https://arxiv.org/abs/1811.00401 which gives a complementary perspective to the analysis associated with Fig 7.2 of Moosavi-Dezfooli’s thesis.
This gives us insight into something that I think Bruno would argue is obvious for high-D spaces — that it’s easy to walk in a straight line from any one image to another while staying in the same classification region.
We need to remember that high-D spaces provide massive freedom for an adversary to find an attack direction.
I think this strongly supports an argument against adv training as a reasonable defense.
You’re never going to patch all of the holes.
And even if you could, hole patching is a horribly inefficient approach.
I like this paper a lot too: https://arxiv.org/abs/1704.03453.
It has some flaws in the methodology, the biggest one being that the projection operator they do to keep the image in [0, 1] bounds destroys the orthogonality constraint when measuring dimensionality and they never test linear independence.
So their estimate of the dimensionality of the adversarial space is not to be believed.
Regardless, this line of thinking is again taking more of a geometric approach.
Understanding the ‘adversarial dimensionality’ is very important for defending against them and for understanding the decision surface in general.
I think we can define adv attacks as any image perturbation that is designed to modify the outcome of the classifier in a specific (i.e. non-random) way.
As you point out, adversarial attacks will always exist.
And as you point out, with my broad definition they are not all interesting.
In my opinion (which I discuss in my own paper) there are two key components to adversarial examples that make them interesting: 1) the size of the perturbation and 2) the semantic content of the perturbation.
These two things are related.

An example of interesting semantic content could be a stroke in the middle of a 0 to make it look like an 8, or [putting wheels on a canoe]{https://johnnygraphicadventures.files.wordpress.com/2014/05/104_0392.jpg?w=1054&h=790} to trick the classifier into thinking it is a bicycle.
This is a fairly reasonable mistake, and therefore not interesting.
If the perturbation were extremely tiny, to the point where it’s basically imperceptible to a human, then it would be more interesting, because the network should not care so much about any perturbation that is so small.
The most interesting thing, though, is that we can make mostly nonsense and tiny perturbations to trick the classifier.
Not only this, but these nonsense perturbations are consistently the ones that we find with gradient based methods.
So yes, while adv attacks will always exist, once they gets big enough we can just call them reasonable, and the same goes for once they looks sufficiently semantically interesting.

As an aside: I think the common assumption is that we find nonsense perturbations because those directions are more optimal than semantically meaningful perturbations.
I mostly agree with this.
There’s a counter argument that semantically meaningless perturbations are just more common and not necessarily more optimal.
However, a lot of papers have found that successful adv defenses (e.g. my paper, here, and here) result in more semantically meaningful adv attacks that are also larger in magnitude.
If you think about it, it makes sense that nonsense perturbation directions that are never in the training set can get you closer to the decision boundary than semantically interesting perturbation directions.

### Gradients
So how does one construct such an attack?
Of course there are many methods, and they can roughly be categorized by how much access they have to the network they are attacking.
It is possible to perform such an attack by only knowing the general goal of the network (e.g. "classify images"), but the most successful attacks have direct access to the network.
This direct access allows the attacker to compute _gradients_, which ideally result in an exact signal for the smallest possible perturbation required to trick the network.

The gradient is a fairly complicated and unfortunately overloaded term in mathematics.
Luckily, though, we only really need the intuition behind it to understand how an attacker would use a gradient to construct an input perturbation.



<!---  high-level explination (point to blog?)   -->
<!---  explain a gradient  -->

### section title
Scientists use a large variety of different types of models to describe neurons in both neuroscience as well as artificial intelligence research.
One way to organize the models is by their level of abstraction, which is describes the tradeoff between biological accuracy and computational complexity.
Indeed, one major goal of computational neuroscience is to discover the level of abstraction that has minimal complexity while still emulating the behavior of biological organisms.
As I exaplained in my [previous post]({{site.url}}/posts/2021/05/response-geometry/), a neuron's response surface geometry can help us differentiate between models and explain their selectivity and invariance properties.
In this post I will demonstrate how the response surface geometry can also predict a neuron's _adversarial robustness_, which I will explain in more detail later but for now think of it as the neuron's ability to provide a consistent output for insignificant changes in its inputs.

### Why are real neurons recurrent?
<!--- linear, nonlinear, & recurrent models explinations  -->
As usual, I want to start with the most simple neuron model -- a _linear_ model.
This type of neuron performs a weighted sum of its inputs to produce its outputs.

<!--- recurrence can cause response-contour bending  -->


### Response geometry determines adversarial attack directions
<!---  attacks will be orthogonal to contours  -->

<!--- constraining the search space  -->

As a running example lets consider a neural network that takes images as inputs and produces labels for those images as output.
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
As a starting point, lets consider a linear neuron, which has straight and equally-spaced iso-response lines.
The horizontal axis is the neuron’s MEI, or maximally exciting image, which was determined to be the image that causes the largest output.
If we perturb an input orthogonal to the iso-response lines, then the activation maximally changes.
If we go in the direction of the MEI (to the right in this case) then the activation maximally grows, and vice versa for the opposite direction.

### Deep neural networks
In reality we consider situations where neural networks perform interesting tasks, which requires many neurons to be assembled into a layered structure.
If we expand our previous example to consider a two-layer network, as shown below, we can appreciate how the importance of iso-response contour shape.

### Curvature and adversarial defense
<!--- MNIST & CIFAR results  -->

### Wrapup
<!--- wrapup  -->

[adversarial-images]: /images/adversarial-robustness/adversarial-robustness.png
