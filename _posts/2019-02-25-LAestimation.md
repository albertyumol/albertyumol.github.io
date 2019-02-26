---
title: "Length and Area Estimation in Images"
date: 2019-02-25
tags: [image processing, discrete calculus, edge detection]
header:
  image: "/images/julia2.gif"
excerpt: "Two dimensional Estimations using Scilab and ImageJ"
mathjax: true

---

<div id="fb-root"></div>
<script async defer src="https://connect.facebook.net/en_US/sdk.js#xfbml=1&version=v3.2"></script>

More and more of our daily activites are becoming automated and digitized. Current technology relies a lot on computer data and its processing to make efficient decisions and solutions. One of such technologies involves optical sensing devices and and smart camera systems. These technologies revolutionized the way we look at things from the very small to those that exceeds our human proportions by orders of magnitude.

In field of medical technology, image processing is a milestone for capturing live and real time images of minute cells. In the field of space exploration we have estimated the areas of the craters of the moon, approximated the heat of the sun, and even counted the very stars that made our heavens. 

This post is a taste of those applications in a VERY simplified manner. Same concepts, same math, minus the proportions (well not really). 
This is all about area estimation using pixel coordiantes.

There are a lot ways in which an area can be estimated given an image. The most basic approch would be directly counting the pixels that lie within a boundary and by sorting out the pixels by the magnitude of its intensity. But that's only one way of doing it. A more 'mathematical' and elegant approach is by using Green's Theorem. This theorem basically relates a double integral to a line integral [1]. 

For some continuous function $$F_{1}$$ and $$F_{2}$$ (with continuous partial derivatives) that contains a region R, Green's Theorem relates that:

<img src="{{ site.url }}{{ site.baseurl }}/images/Equation1_LA.png" alt="hi">
















<div class="fb-comments" data-href="https://albertyumol.github.io/" data-numposts="5"></div>