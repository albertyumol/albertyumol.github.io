---
title: "Length and Area Estimation in Images"
date: 2019-02-25
tags: [image processing, discrete calculus, edge detection]
header:
  image: "/images/julia2.gif"
excerpt: "Two dimensional Estimations from Images using Scilab and ImageJ."
mathjax: true

---

<div id="fb-root"></div>
<script async defer src="https://connect.facebook.net/en_US/sdk.js#xfbml=1&version=v3.2"></script>


More and more of our daily activites are becoming automated and digitized.
Current technology relies a lot on computer data and its processing to make efficient decisions and solutions.
One of such technologies involves optical sensing devices and smart camera systems.
These technologies revolutionized the way we look at things from the very small to those that exceed our human proportions by orders of magnitude.

In the field of medical technology, image processing is a milestone for capturing live and real time images of minute cells. 
In space exploration, we have estimated the areas of the craters of the moon, approximated the heat of the sun, and even counted the very stars that made our heavens. 

This $$blog post$$ is a taste of those applications in a VERY simplified manner. 
Same concepts, same math, minus the proportions area estimation using pixel coordinates.

There are a lot ways in which an area can be estimated given an image. 
The most basic and most rigorous approach would be directly counting the pixels that lie within a boundary and by sorting out the pixels by the magnitude of its intensity. 
But that's only one way of doing it. A more 'mathematical' and 'elegant' approach is by using [Green's Theorem](https://en.wikipedia.org/wiki/Green%27s_theorem) (recall basic Calculus). 
This theorem basically relates a double integral to a line integral [1]. 

For some continuous function $$F_{1}$$ and $$F_{2}$$ (with continuous partial derivatives) that contains a region $$R$$, Green's Theorem relates that:

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/LA/Equation1_LA.png" alt="Equation 1" class="center">
{: refdef}

Suppose that $$F1=0$$, $$F2=x$$ and $$F1=−y$$, $$F2=0$$, then at the region $$R$$ and at the contour boundary $$C$$, the equation above becomes,

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/LA/Equation2_LA.png" alt="Equation 2" class="center">
{: refdef}

These double integrals give us the area of the region $$R$$ to the right and to the left, respectively. Adding the two, 

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/LA/Equation3_LA.png" alt="Equation 3" class="center">
{: refdef}

In discrete form,

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/LA/Equation4_LA.png" alt="Equation 4" class="center">
{: refdef}

For the first part, I used Green's Theorem to find the area of regular shapes. 
Using *Scilab*, I generated a circle with radius of $$0.7 units$$, a rectangle with length $$1.4 units$$ and width $$1 unit$$, a tringle with height $$0.5 unit$$ of and base of $$1 unit$$. 
Shown below are the synthetic images.

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/LA/Image1_LA.png" alt="Generated Synthetic Shapes" class="center">
{: refdef}

Before we can apply Green's Theorem, the edge of the shapes should be determined first. 
In *Scilab*, the module **SIVP** (Scilab Image and Video Processing toolbox) has five readily available edge detection algorithms [2]:


**sobel**

<small>Detects edges, using the sobel gradient estimator.  
It isbased on convolving the image with a small, separable, and integer-valued filter in the horizontal and vertical directions [3].</small>

**prewitt**

<small>Detects edges, using the prewitt gradient estimator. 
Technically, it is a discrete differentiation operator, computing an approximation of the gradient of the image intensity function [4].</small>

**log**

<small>Detects edges, using the the Laplacian of Gaussian method. 
$$\sigma$$ is the standard deviation of the Log filter and the size of the Log filter is $$nxn$$, where $$n = ceil(\sigma*3)*2+1$$. The default value for $$\sigma$$ is $$2$$.</small>

**fftderiv**

<small>Detects edges, using the FFT gradient method, default $$\sigma = 1.0$$</small>

**canny**

<small>Detects edges in *im*, using Canny method. *thresh* is a two-element vector, in which the fist element is the low threshold and the second one is the high threshold. 
If *thresh* is a scalar, the low threshold is $$0.4*thresh$$ and the high one is *thresh*. Besides, *thresh* can not be negative scalar. 
$$\sigma$$ is the Aperture parameter for Sobel operator, which must be 1, 3, 5 or 7; default $$thresh = 0.2$$; default $$\sigma = 3$$.</small>

Green theorem is then applied to the edges of the shapes with the following code:

{% highlight scilab %}
//Opens the image to be processed
Circle = imread('C:\Users\csrc-lab03\Desktop\yakal5.bmp');
Circle = rgb2gray(Circle)

//Use edge() function sobel
edge_sobel = edge(Circle,'canny'); 
imshow(edge_sobel);
imwrite(edge_sobel,'Circle_sobel.bmp');

//Edge pixel coordinates
[i_x,i_y] = find(edge_sobel); 

//Locate centroid coordinates (in pixels) using average.
//sum([]) adds the elements in []. size([],2) gets the number of columns.
x_center = sum(i_x)/size(i_x,2);
y_center = sum(i_y)/size(i_y,2);

//Calculate position magnitude r and angle theta
x_loc = i_x - x_center;
y_loc = i_y - y_center;
r = sqrt((x_loc).^2+(y_loc).^2);
theta = atan((y_loc),(x_loc));

//Sort values according to increasing angle. gsort() function is used.
//theta_sorted is the sorted angle array, and is its corresponding index in the unsorted array
//'g' sorts all elements in 'i' increasing order

//Contains the values x and y pixels (transposed).
[theta_sorted, index] = gsort(theta','g','i');
xy_array = [x_loc;y_loc]' 

//Sort x and y coordinates according to increasing angle.
xy_sorted = xy_array(index,:);
x_sorted = xy_sorted(:,1);
y_sorted = xy_sorted(:,2);

//Apply Green's finction in x_sorted and y_sorted. Obtain summations xdy and ydx for area computation.
xdy = sum(x_sorted(1:size(x_sorted,1)-1).*y_sorted(2:size(y_sorted,1)));
ydx = sum(y_sorted(1:size(y_sorted,1)-1).*x_sorted(2:size(x_sorted,1)));
Area_Comp = 0.5*(xdy - ydx);

//Compute Theoretical Area
Area_Theo = length(find(Circle==255));

//Compute % error
Error = 100*abs(Area_Comp - Area_Theo)/Area_Theo;
disp(Area_Comp); disp(Area_Theo); disp(Error);
{% endhighlight %}

The comparison of how the five edge detection algorithms performed is shown,

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/LA/Image2_LA.png" alt="Actual Results" class="center">
{: refdef}

Below is a table showing the summary of the results of the calculated area of the shapes using Green's theorem with different edge detection algorithms. 

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/LA/Table1_LA.png" alt="Edge Detection Algorithm Results Comparison" class="center">
{: refdef}

From visual inspection of the images, the prewitt, fftderiv and canny produced the thinner and more accurate edge detection compared to the others. 
These visual comparisons are backed up by the low percent error of the computed area to the analytical area of the shapes as can be inferred from the Table. 
Overall, the **canny** algorithm seems to outperform all the other given that it produced the least error value.

To test this, I went to **Google Earth** (left) and **Google Maps** (right) and searched my beloved dormitory in UP (Yakal Residence Hall). 
The images below show the sattelite view of the dormitory.

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/LA/Image3_LA.png" alt="Yakal Dormitory" class="center">
{: refdef}

I uploaded the **Google Maps** image to **Paint** (Microsoft), whiten the area of the building and blacken everything else as shown:

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/LA/Image4_LA.png" alt="Extracted Image in Paint" class="center">
{: refdef}

Applying **Green's Theorem** to find the area and comparing the five edge detection algorithms, the results are as follow:

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/LA/Image5_LA.png" alt="Extracted Image Results" class="center">
{: refdef}

The table below shows the summary of the results of the calculated area of the shapes using **Green's Theorem** with the five different edge detection algorithms. 

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/LA/Table2_LA.png" alt="Edge Detection Algorithm Results Comparison 2" class="center">
{: refdef}

I found the actual area of the dormitory by getting the convex hull of the building in **Google Maps**. 
This is done by getting the smallest number of points to enclose the area. 
Google automatically calculated the area for me, so no sweat. 
The table above is fairly consistent from the results of the regular shapes. 
Indeed the **canny** algorithm bested the other four. 
The offset of the canny method to the actual area is about $$43 m^2$$ which is roughly about four standard rooms. 
Not bad for my purpose of approximating areas with Green's theorem.
Most errors would come from me being a human huhu i.e. misaligning in covering area in **Google Maps**.

For those people who want to skip rigorous math and don't have much time, an alternative method is by using ImageJ and a straight edge.
I scanned my decade old DOST ATM card together with a reference ruler (for scaling and direct measurement). 
I definitely look older 10 years ago.

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/LA/Image6_LA.png" alt="My DOST ID" class="center">
{: refdef}

I uploaded this image to ImageJ, a free open source software for measuring microscopic images. 
I drew a straight line across the horizontal and set the scales as shown:

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/LA/Image7_LA.png" alt="DOST in ImageJ" class="center">
{: refdef}

After setting the scales, I know that the area of my card is $$45.9 cm^2$$ by physical measurements. 
I then used the *free hand selection tool* to draw a polygon that fits around the sides of my card. 
I clicked the measurements and it gave an area of $$45.842 cm^2$$ which is $$0.12%$$ away from the actual value. 
It was accurate enough even though I was not even making tremendous effort on making the shape really fit the sides line to line.
​

References:

[1] M. Soriano, “Length and Area estimation in images,” Applied Physics 186 Activity Hand-outs, 2014.

[2] Scilab Image Processing. Retrieved from:
[http://siptoolbox.sourceforge.net/doc/sip-0.7.0-reference/edge.html](http://siptoolbox.sourceforge.net/doc/sip-0.7.0-reference/edge.html)

[3] Wikipedia. Sobel Operator. Retrieved from:
[http://siptoolbox.sourceforge.net/doc/sip-0.7.0-reference/edge.html](https://en.wikipedia.org/wiki/Sobel_operator)

[4] Wikipedia. Prewitt Operator. Retrieved from:
[http://siptoolbox.sourceforge.net/doc/sip-0.7.0-reference/edge.html](https://en.wikipedia.org/wiki/Prewitt_operator)

[5] Barteezy's Applied Physics Experience. Retrived from: [http://siptoolbox.sourceforge.net/doc/sip-0.7.0-reference/edge.html](https://barteezy.wordpress.com/2015/09/06/activity-4-length-and-area-estimation-in-images/)

<div class="fb-comments" data-href="https://albertyumol.github.io/" data-numposts="5"></div>