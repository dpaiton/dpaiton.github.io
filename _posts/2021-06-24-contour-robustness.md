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

### Adversarial attacks
<!---  high-level explination (point to blog?)   -->
<!---  explain a gradient  -->

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

