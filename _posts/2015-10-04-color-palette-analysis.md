---
title: "Palette: Image Color Palette Experiments"
---

![Quantized Image]({{site.url}}/assets/quantized_1.png)

## Motivation
After seeing a [Disney blog post](http://blogs.disney.com/oh-my-disney/2015/05/11/these-disneypixar-palettes-are-the-most-aesthetically-pleasing-things-youll-see-all-day/#Brave) about the different color palettes in Pixar films, I started to wonder if I could write a program to find the main color palette of an image and then make changes to the palette.

For a live demo, click [here](/palette.html)

## First Steps
The process of generating a color palette from an image is very similar to the process of [color quantization](https://en.wikipedia.org/wiki/Color_quantization):

> In computer graphics, **color quantization** or color image quantization is a process that reduces the number of distinct colors used in an image, usually with the intention that the new image should be as visually similar as possible to the original image.

There are many different clustering algorithms that can be used to reduce the number of colors in image, but for this project I'll be using the [K-means algorithm](https://en.wikipedia.org/wiki/K-means_clustering).

![Sample Image]({{site.url}}/assets/sample_1.jpg)

Each pixel in the original image has a color with three components: red, green, and blue. Each of these components is a number between 0 and 255. For the rest of this project, red will be mapped to x, green will be mapped to y, and blue will be mapped to z in 3D coordinates.

Here is what all of the pixels look like for our image:

![Sample graph]({{site.url}}/assets/graph_1.png)
