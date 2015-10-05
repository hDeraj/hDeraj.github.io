---
title: "Palette: Image Color Palette Experiments"
---

![Quantized Image]({{site.url}}/assets/palette_cover.jpg)

## Motivation
After seeing a [Disney blog post](http://blogs.disney.com/oh-my-disney/2015/05/11/these-disneypixar-palettes-are-the-most-aesthetically-pleasing-things-youll-see-all-day/#Brave) about the different color palettes in Pixar films, I started to wonder if I could write a program to find the main color palette of an image and then make changes to the palette.

For a live demo, click [here](/palette.html)

We will be using this sample image for the rest of the post.

![Sample Image]({{site.url}}/assets/sample_1.jpg)

## First Steps
The process of generating a color palette from an image is very similar to the process of [color quantization](https://en.wikipedia.org/wiki/Color_quantization):

> In computer graphics, **color quantization** or color image quantization is a process that reduces the number of distinct colors used in an image, usually with the intention that the new image should be as visually similar as possible to the original image.

There are many different clustering algorithms that can be used in color quantization, but for this project I'll be using the [K-means algorithm](https://en.wikipedia.org/wiki/K-means_clustering).


## Some Background on Images
Each pixel in an image has a color with three components: red, green, and blue. Each of these components is a number between 0 and 255. So for example, we can represent the color red by <kbd>(255, 0, 0)</kbd> : this means that the pixel has a red value of 255, and blue and green values of 0. I'll refer to these triples using x, y, and z for red, green, and blue respectively.

Since each pixel has x, y, and z values, we can plot them in 3d. Here is what all of the pixels look like for our sample image:

![Sample graph]({{site.url}}/assets/graph_1.png)

Keeping this spatial representation of pixels in mind will be helpful for understanding how the K-means algorithm works.

## K-means Clustering
Now that we can represent each pixel using x,y,z coordinates, it's time to use the K-means algorithm to find clusters of similar colors. The algorithm consists of a setup stage, and a loop stage:

### Setup:
- Pick a number <kbd>k</kbd> to be the number of clusters to search for.
- Randomly choose <kbd>k</kbd> points from the image to initialize the clusters.

### Loop:
- For every pixel, calculate the distance to each of the <kbd>k</kbd> cluster points, and assign the pixel to the cluster it is nearest to.
- Change the x, y, and z values for each cluster point to be the average of all of their assigned points
- Repeat these steps until the cluster points no longer change, or until a certain amount of iterations have occured.

<div class="alert alert-warning" role="alert">Note that the loop stage has to go through every single pixel in the image multiple times. This can take a **very** long time for large images.</div>

One cool feature of the K-means algorithm is that it is *guaranteed* to reach a local* optimum (it is not guaranteed to reach the global optimum).

## Quantization
After running the K-means algorithm, we are left with <kbd>k</kbd> points which represent the centers of each cluster, as well as the pixels that are assigned to each of the points. The quantization step is relatively straightforward: we redraw the original image, but replace each pixel's color with that of its nearest cluster point.

Here is what it looks like for our sample image when <kbd>k = 3</kbd> :

![Quantized Image]({{site.url}}/assets/quantized_1.png)

The resulting image is visually very similar to the original photo, and the colors depict those in the original fairly accurately. One downside is that the resulting colors are "washed out", or not very bright. To understand why, we have to look into what the K-means algorithm actually did.

For <kbd>k = 3</kbd>, here is what the 3D plot of the pixels looks like:

![Graph K-3]({{site.url}}/assets/graph_k3.png)

Lots of "similar" and "not-so-similar" colors from the original graph got grouped together into one color. Because the K-means algorithm averages all of them together, the resulting colors will not be as bright as the original ones.

## Further Enhancements
When running the algorithm, we need to choose some value for <kbd>k</kbd>. To see what kind of effects we can get with different values, I ran the algorithm for a couple more values.

### K = 4
![Quantized K-4]({{site.url}}/assets/quantized_k4.png)
![Graph K-4]({{site.url}}/assets/graph_k4.png)

### K = 5
![Quantized K-5]({{site.url}}/assets/quantized_k5.png)
![Graph K-5]({{site.url}}/assets/graph_k5.png)

### K = 6
![Quantized K-6]({{site.url}}/assets/quantized_k6.png)
![Graph K-6]({{site.url}}/assets/graph_k6.png)

As the value of <kbd>k</kbd> increases, the output of the algorithm gets closer and closer to the original image. The colors also get "brighter" since they are averaging a smaller number of colors which are also closer together.

Just for fun, I also ran it with a much larger value.

<div class="alert alert-danger" role="alert">This <strong>will</strong> take a long time to run.</div>

### K = 23
![Quantized K-23]({{site.url}}/assets/quantized_k23.png)
![Graph K-23]({{site.url}}/assets/graph_k23.png)

I made a simple website where you can try this out with your own images. Try it out and let me know if you have any issues.

[Palette Demo Website](http://hDeraj.github.io/palette)
