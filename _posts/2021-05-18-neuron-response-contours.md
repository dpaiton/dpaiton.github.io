---
title: 'Explaining neurons using response geometry'
date: 2021-05-24
permalink: /posts/2021/05/response-geometry/
tags:
  - response geometry
  - neural system identification
  - deep net interpretability
---

Dylan M Paiton
------

### Neuron selectivity and invariance
An important computational goal of the early visual pathway, from the retina to the primary visual cortex (V1), is to produce a representation of visual features that are useful for downstream tasks, like navigating the world and identifying objects.
In order for a V1 neuron’s output to be decoded by downstream brain areas, it must reliably modulate its response with respect to certain variations in the visual world and remain constant for others.
For example, lets suppose we live in a binary edge world, where all scenes are made up of combinations of oriented edges.
In such a world, images of vertical edges would occur when a column of 1s immediately preceded a column of 0s, or vice versa.
To encode such a world, our brain might want to have a 'vertical edge detector' neuron that signals the presence of any vertical edges.
This means we want our neuron to be _selective_ for the presence of an vertical edge, as opposed to other orientations, but _invariant_ to the phase of the edge, i.e. whether it is 0s followed by 1s or the other way around.
In neuroscience, such a neuron is called a complex cell, and is a standard basic cell type.
It was precisely the discovery of analogous neuron types in the visual cortex of cats that lead scientists to hypothesize that the brain is wired up to perform _computations_ on signals coming from the world.
This discovery occured in the late 1950s, and about twenty years later a scientist named Dr. Kunihiko Fukushima proposed one of the earliest ancestors of the modern day deep neural network, called the neocognitron.
One of the defining elements of the neocognitron were "C-cells", which were designed to be selective for features in the world, but invariant to their position.
In modern artificial neural networks, we often label neurons by what they are most selective for, such as snout detectors, and we explicitly engineer components that create invariance, such as pooling to create invariance to position.
Today scientists have a variety of methods to measure and quantify selectivity and invariance in both biological as well as artificial neurons.
These measurements are extremely important, and are often used to categorize neurons into certain groups.
This blog post aims to explain one such method, which is currently primarily used in neuroscience and relies on characterizing the geometry of a neuron's _response surface_.

### The neural response surface
First, an obvious statement: neurons receive signals as input and produce signals as output.
Let's assume that some target neuron receives input from 100 other neurons, and produces a single output that might be read by any number of downstream neurons.
Going forward, we will define the _target neuron_ as the neuron that we are currently interested in studying, and we will indiciate it mathematically with the index $$k$$.
Lets assume that the target neuron will produce an output for any combination of signals coming from the 100 input neurons.
Or, put another way, the target neuron is a function that maps a 100-dimensional input to a single-dimensional (i.e. scalar) output.
The neural response surface is a description of the neuron's output for all possible combinations of inputs.
In this example the response surface would be 101-dimensional, since we want to keep track of each 100-dimensional input as well as the corresponding 1-dimensional output.

It is impossibly difficult to measure the entire response surface of a neuron, even in the case of simple artificial neural networks.
However, if we assume that the response surface is relatively smooth and regular, then we can still learn a lot about the neuron by looking at cross sections, which are much more tractible to estimate.
This presents our next challenge, which is choosing how to take a cross section.
Just like cutting an onion in half, one type of cross section will reveal very different structure from another one.

![onions]
{: style="text-align: center"}

**fig. 1**
Taking cross sections of complicated surfaces can reveal structure, but the structure looks different depending on the orientation of the cross section.
{: style="text-align: center"}

Instead of input neurons, lets think about a neuron that receives image pixels as input and produces a scalar output.
Individual input images can also be thought of as points in a high dimensional space.
For example, a 10-by-10 image thumbnail has 10\*10=100 pixels.
Therefore, we can equivalently think of the image as a point in a 100-dimensional space.
Each dimension can be represented as an axis, where the position along the axis is the value of the pixel.
If individual pixels are allowed to be numbers between 0 and 1 (like in a grayscale image), then all allowable images exist in a 100-dimensional cube.
Just to really drive this idea home -- every single point in the 100-dimensional cube is also an image.
The next figure has an illustration of a 3D image space with such a 2D cross section drawn in light grey.

![2d-plane]
{: style="text-align: center"}

**fig. 2**
Images are high-dimensional points.
Cross sections can be taken in the high-dimensional space to define a subset of images that are coplanar.
In this illustration, each yellow axis represents the value of a pixel for a three-pixel image.
The grey sheet represents a plane, or cross section.
The purple and blue arrows are vectors that define the plane.
The red arrow points in the remaining direction, which is perpendicular to the plane.
{: style="text-align: center"}

Here's a roadmap of what is to come:
To explore the response geometry of our target neuron, we will first take a two-dimensional cross section of the high-dimensional input space.
Next we will discretize the cross section, that is to say we will sample the cross section with a tiling of points evenly spaced from the minimum allowable pixel value to the maximum.
Then we will measure the output of the neuron for each of the images selected from the discretization process.
Finally, we will measure the _curvature_ of the response surface to quantify selectivity and invariance of the neuron.

### Choosing a cross section
We are interested in the behavior of the target neuron for images that it cares about, or ones it would see in its natural environment.
As such, we shouldn't use just any old cross section to analyze a neuron, but instead cross sections that contain images (i.e. _points_) that are relevant for that neuron.
The next figure illustrates how each point in the grid can be reshaped to be visualized as an image.
A few of the points around the edges are displayed as images.
Notice how these images have some structure to them, and are systematic in how they look.
For any given cross section, all of the images are [coplanar](https://en.wikipedia.org/wiki/coplanarity), which is to say that they can all be described as a [linear combination](https://en.wikipedia.org/wiki/Linear_combination) of two exemplar images.


![points-to-images]
{: style="text-align: center"}

**fig. 3**
We discretize an image cross section (i.e. plane) into a two-dimensional grid and collect the points to define a dataset
of images.
The black arrows indicate orthogonal (i.e. perpendicular) vectors that can be used to define the plane.
All images in the plane can be described as a linear combination of the two orthogonal image vectors indicated in black.
The origin for all of the planes we consider here will be a zero-contrast image, in other words a uniform gray image.
{: style="text-align: center"}

In addition to constraining our input space to be coplanar, we are also going to constrain our output space to simplify the analysis.
Specifically, we are going to look at the curvature of the _iso-response contours_ of the neuron.
These contours are connected points that are all on the plane and all produce the same output from the neuron.
To see the iso-response contours, we will compute the neuron’s output (i.e. _activity_) for all of the cross section images and then recolor the points according to the activity.


As I said before, we want to pick a plane that is relevant for our neuron.
To do this we define one of the two plane axes as the neuron's _maximally exciting image (MEI)_, which is an image that was optimize to look like a feature in the world that is most interesting to the neuron.
There are a lot of ways to find a neuron's MEI in neuroscience and in artificial neural network research, but for now lets leave asside how the image was produced and just assume it is representative of what the neuron likes most.
Going forward, we will represent the MEI with the symbol $$\Phi_{k}$$, where the $$k$$ tells us it is the MEI for the $$k$$th neuron in our assembly.
For now lets just assume that the other axis is any random orthogonal image, but we will change that later.

![choosing-the-right-plane]
{: style="text-align: center"}

**fig. 4**
The horizontal axis for an image plane is always defined to be the target neuron's maximally exciting image, or MEI.
{: style="text-align: center"}

### Plotting the contours
We can now color the points according to the neuron’s activity, $$a_{k}$$.
Finding the iso-response contours is easy, we just collect the output values into a fixed number of bins, and the bin boundaries will lie along iso-response contours.
Here's what it would look like for a very simple (linear) neuron:

![linear-contours]
{: style="text-align: center"}

**fig. 5**
The iso-response contours for a linear neuron are straight & orthogonal to the neuron's MEI, $$\Phi_{k}$$.
In this image, $$\Phi_{j}$$ indicates the MEI for some other neuron and $$\nu$$ indicates the chosen orthogonal axis.
The iso-response contours will be straight regardless of the chosen direction for $$\nu$$.
{: style="text-align: center"}

Notice how the lines are straight & orthogonal to $$\phi_{k}$$.
This will be true for any linear system, and it means that the neuron is insensitive to orthogonal perturbations away from it's preferred input.
What happens if we apply a pointwise nonlinearity to our neuron's output?
This is exactly the type of operation that is uesd in almsot all standard deep neural networks: a linear operation followed by a pointwise nonlinearity.
For example, in the next figure we include the _Rectified Linear Unit (ReLU)_ nonlinearity, which sets all values below some threshold to zero and then acts as an identity mapping (i.e. input equals output) for all values above that same threshold.

![pointwise-neurons]
{: style="text-align: center"}

**fig. 6**
The iso-response contours for a pointwise nonlinear neuron are still straight & orthogonal to the neuron's MEI.
The nonlinearity can only change the spacing of the lines, or set values to zero.
{: style="text-align: center"}

We can see that the iso-response contours are still straight.
This is because the nonlinearity is applied after the linear operation, and so the nonlinearity can only change the spacing
between the lines.
This is true for all types of pointwise nonlinearities, which we demonstrate by also including a sigmoid nonlinearity.

### Straight vs. curved contours
Other types of artificial neurons, as well as biological neurons, tend to have more complicated iso-response contoures.
Specifically, they tend to have _curvature_ in the sense that they bend towards or away from the origin.
Since linear neurons produce straight contours, it is reasonable to think about curvature as indicating a deviation from linearity -- more curvy contours indicate less linear the input-output functions.
Additionally, lets differentiate between outward curvature, which means it's bending away from the origin, and inward curvature, which means it is bending towards the origin.
The curvature is significant because it tells us specifically about neuron selectivity and invariance.
To understand that better, lets consider a few different types of coplanar perturbations that we can make to an input image.

Lets choose somewhere along the horizontal axis as a starting point and list some options for perturbation directions:
- Right, towards the neuron's MEI, then the neuron's activity will grow
- Left, away from the neuron's MEI, then the neuron's activity will shrink
- Up, orthogonal to the neuron's MEI
- Along the iso-response contour, which will not change the neuron's output

![perturbation-directions]
{: style="text-align: center"}

**fig. 7**
Visualizations of four (of many) simple pertubation directions.
For each direction, assume we start with the image depicted by the square, then perturb to the pentagon, then again to the circle.
Straight contours are indicated in shades of purple, outward bending in shades of red, and inward bending in shades of blue.
Darker colors indicate higher neural activation.
{: style="text-align: center"}

For linear neuron & pointwise nonlienar neurons, the last two directions are equal.
But they are not equal for neurons with bent iso-response contours.
What does this tell us?
Lets look at one in particular to start, the 'up' perturbation for straight and outward bending contours.

![up-perturbation]
{: style="text-align: center"}

**fig. 8**
The 'up' perturbation direction reveals an important difference between neurons with straight contours and neurons with outward curved contours.
{: style="text-align: center"}

Again lets consider the pointwise nonlinear neuron.
What happens to the output if we take a fixed point on the $$\Phi_{k}$$ axis and then perturb orthognally in the up direction?
Nothing; as expected, the response is flat.
However, the other neuron the curvature indicates a decrease in output, $$a_{k}$$, as we increase our perturbation in the $$\nu$$ direction.
We call this _response attenuation_.
This means the neuron is selective against orthogonal perturbations away from its preferred stimulus.
In other words, outward bending contours reveal that the target neuron’s output contains more information about how close the stimulus is to its MEI than straight contours.
This is important if we consider what the "MEI" means.
In artificial neural networks, we use the "MEI" to label the neuron.
The biological analog for an "MEI" is the neuron’s receptive field, which neuroscientists also use to label or identify neurons.
For linear and pointwise nonlinear neurons, we can perturb images along any direction that is orthogonal to the neuron’s preferred stimulus and the response will not change.
However, for neurons with outward facing iso-response contour curvature, orthogonal perturbations will result in response attenuation.
Therefore, the neuron outputs have a higher degree of correspondence to their preferred stimulus label, and thus their output carries more relevant information about the input.
Another way to say all of this is to say that the outward curvature indicates increased _selectivity_, where the amount of selectivity is proportional to the amount of curvature.

As another example of the utility of this analysis, lets look at one more case, which is the perturbations that are along the iso-response contour.
In this case, lets compare the neuron with flat iso-response contours to the one with inward curved iso-response contours.
If we look at the images that lie along evenly spaced intervals of the contours we can see a clear difference between the two neuron types.
By definition, since these images lie along the iso-response contour, the output of the neuron is equal for all of them.
This means that the bottom cell has phase _invariance_, while the top cell does not.

![contour-perturbation]
{: style="text-align: center"}

**fig. 9**
The 'contour' perturbation direction reveals an important difference between neurons with straight contours and neurons with inward curved contours.
The generated images are called _Gabor functions_, and are often used as an analogy for V1 neuron MEIs.
Images produced along the contour for the top neuron cause an increase in contrast for the orthognal direction.
In this case, it just looks like darks are getting darker and whites are getting whiter, although if you inspect it carefully you should see that it is not happening equally for all vertical segments.
However, image produced along the contour for the bottom neuron shift in phase.
{: style="text-align: center"}

## Wrap up
There is much more that we can learn by looking at the curvature of the iso-response surfaces, which I will discuss in upcoming posts.
For now, the big takeaways are:
- linear and pointwise nonlinear neurons have flat iso-response contours, regardless of the nonlinearity used
- outward-curved contours indicate selectivity, and the amount of curvature will tell us how selective the neuron is
- inward-curved contours indicate invariance, which is a property of biological complex cells
For more published scientific articles on this topic, I recommend reading [my own paper](/publication/2020-11-02-selectivity-and), as well as the work of [James Golden et al.](http://jamesgolden.net/wp-content/uploads/2017/10/golden_vilankar_wu_field_conjectures_nonlinear_geometry_vision_research_2016_preprint.pdf), which was published in the journal _Vision Research_ in 2016.

[2d-plane]: /images/response-geometry/2d-plane.png
[points-to-images]: /images/response-geometry/points-to-images.png
[choosing-the-right-plane]: /images/response-geometry/choosing-the-right-plane.png
[linear-contours]: /images/response-geometry/linear-contours.png
[pointwise-neurons]: /images/response-geometry/pointwise-neurons.png
[onions]: /images/response-geometry/onions.png
[up-perturbation]: /images/response-geometry/up-perturbation.png
[contour-perturbation]: /images/response-geometry/contour-perturbation.png
