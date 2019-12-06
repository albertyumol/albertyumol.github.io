---
title: "Selective Colorization in GIMP - A Tutorial"
date: 2019-03-09
tags: [Image Processing]
header:
  #image: "/images/julia2.gif"
  #image: "/images/backdrop7.png"
  teaser: "/images/gimp/desaturation.JPG"
excerpt: "My real first experience with an image editor software."
mathjax: true

---

<div id="fb-root"></div>
<script async defer src="https://connect.facebook.net/en_US/sdk.js#xfbml=1&version=v3.2"></script>

DISCLAIMER:

I have been reading a lot of Jessica Zafra's lately. I might have acquired her angsty writing style. Regardless, I am a nice person in general.


I was not born with artistically coordinated hands. I can't even draw passable stick figures. My right hand sketches are practically as bad as my left. Having a physics background only worsened this 'skill' as I was trained to assume and over simplify 3D moving objects as 1D dots and particles connected with hasty crooked arrows. I find art fun but I guess it's not mutual.

However, in my reality, I am self-proclaimed hipster. And being a hipster means I need to try to be as artsy as possible. From dressing as outrageous as I can to dying my hair neon. Art has been where I vent most of my self expression.

I always wanted to be good at something artistically because obviously art is fun. This is my one step of fulfilling my dream of becoming a future Van Gogh. And I'm not pertaining to our sanity.

Most of the time, I pester my photoshop savvy friends for the most menial photo editing tasks i.e. removing my ever present freckles and blemishes. Don't get me wrong. I am not vain at all. It's just that cameras don't hit my face at the right angles. Only till now have I built courage to explore image manipulation and finally do it myself. Surprisingly, it was not at all hard. I would not write a blog if it is so. Trust me.

The first step if you want to be a 'manipulator' is finding a reliable medium (software) to manipulate with. If you have been following my previous posts, you would know that I abhor paid 'utility' software. I am not entirely for free platforms like the manipulative facebook.

The software to use is out of the question. I won’t even dare to touch Adobe products. Those wrecked overpriced and RAM hogging monsters. Of course I am biased to Free Open Source Software and the best option for all of us is [GIMP](https://www.gimp.org/).

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/gimp/gimp_logo.png" alt="gimp" class="center">
{: refdef}

<blockquote>
<small> GIMP is a cross-platform image editor available for GNU/Linux, OS X, Windows and more operating systems. It is free software, you can change its source code and distribute your changes. Whether you are a graphic designer, photographer, illustrator, or scientist, GIMP provides you with sophisticated tools to get your job done. You can further enhance your productivity with GIMP thanks to many customization options and 3rd party plugins. </small>
</blockquote>

Shoutout to the developers and volunteers of GIMP. I feel and applaud you. Long live open-source evangelists. Let’s all fight for free technology for the people.

I was curious about their logo. discuss

Ok. Enough chitchat and let's get down to the dirty work.

Selective colorization for our purpose is defined as selecting/highlighting an object from an image with the background in grayscale. Like this pretty photo with a pretty object of interest - my face.

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/gimp/desaturation.JPG" alt="Me in Tower Bridge London" class="center">
{: refdef}

**Step 1.** Select an image of interest (preferably with outstanding and loud colors for easier separation) and open it with GIMP.
Here is my sample image:

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/gimp/base_image1.JPG" alt="Kids in Trafalgar Square" class="center">
{: refdef}

This photo was taken at some lazy afternoon at Trafalgar Square in Central London. I was so tempted to lift the star war eme to terrorize the naive kids. Here is what the image looks like when imported in GIMP.

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/gimp/image_in_gimp.png" alt="Kids in Trafalgar Square" class="center">
{: refdef}

**Step 2.** Duplicate image from **Layer > Duplicate Layer**.

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/gimp/duplicate_layer.png" alt="duplicate_layer" class="center">
{: refdef}

**Step 3.** Click on the duplicate image and Desaturate (meaning to remove colors) it from **Colors > Desaturate > Desaturate** and select the mode as **Average (HSI Intensity)**. You should see that the duplicate image turns gray.

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/gimp/duplicate_layer_select.png" alt="duplicate_layer_select" class="center">
{: refdef}

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/gimp/desaturate_desaturate.png" alt="desaturate_desaturate" class="center">
{: refdef}

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/gimp/average_hsi.png" alt="desaturate_desaturate" class="center">
{: refdef}

**Step 4.** Still manipulating with this gray image, we need to add a layer mask. Click through **Layer > Mask > Add Layer Mask** and select **White (full opacity)** then add.

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/gimp/add_layer_mask.png" alt="desaturate_desaturate" class="center">
{: refdef}

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/gimp/white.png" alt="white" class="center">
{: refdef}

**Step 5.** Next, in the toolbox on the left side, select the eraser tool. At the bottom of the toolbox, make sure that the foreground palette is set to white while the background is black.

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/gimp/eraser.png" alt="eraser" class="center">
{: refdef}

**Step 6.** Zoom in to your object of interest and with the eraser tool brush through to unmask the color.

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/gimp/zoom.png" alt="zoom" class="center">
{: refdef}

I recommend using 400% zoom as it is easier to cleanly resurface the color at the edges of the image.

This is the image that I produced after the selective colorization process.

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/gimp/base_image_desaturated.JPG" alt="base_image_desaturated" class="center">
{: refdef}

Here are other photos that I had fun to play with:

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/gimp/china_layas.JPG" alt="china_layas" class="center">
{: refdef}

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/gimp/anti_china_kids.JPG" alt="anti_china_kids" class="center">
{: refdef}

But hey it's fun! Go on, try it yourself and comment down your best images.

Want to collaborate? Message me in [LinkedIn](https://ph.linkedin.com/in/albertyumol).

<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<script>
  (adsbygoogle = window.adsbygoogle || []).push({
    google_ad_client: "ca-pub-6410209740119334",
    enable_page_level_ads: true
  });
</script>

<div class="fb-comments" data-href="https://albertyumol.github.io/" data-numposts="5"></div>
