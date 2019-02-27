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


More and more of our daily activites are becoming automated and digitized. \n
Current technology relies a lot on computer data and its processing to make efficient decisions and solutions. \n
One of such technologies involves optical sensing devices and and smart camera systems. \n
These technologies revolutionized the way we look at things from the very small to those that exceeds our human proportions by orders of magnitude.

In field of medical technology, image processing is a milestone for capturing live and real time images of minute cells. In the field of space exploration we have estimated the areas of the craters of the moon, approximated the heat of the sun, and even counted the very stars that made our heavens. 

This post is a taste of those applications in a VERY simplified manner. Same concepts, same math, minus the proportions (well not really). 
This is all about area estimation using pixel coordiantes.

There are a lot ways in which an area can be estimated given an image. The most basic approch would be directly counting the pixels that lie within a boundary and by sorting out the pixels by the magnitude of its intensity. But that's only one way of doing it. A more 'mathematical' and elegant approach is by using Green's Theorem. This theorem basically relates a double integral to a line integral [1]. 

For some continuous function $$F_{1}$$ and $$F_{2}$$ (with continuous partial derivatives) that contains a region $$R$$, Green's Theorem relates that:


{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/Equation1_LA.png" alt="Equation 1" class="center">
{: refdef}

Suppose that $$F1=0$$, $$F2=x$$ and $$F1=−y$$, $$F2=0$$, then at the region $$R$$ and at the contour boundary $$C$$, using first equation,

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/Equation2_LA.png" alt="Equation 2" class="center">
{: refdef}

The double integral gives us the area of the region R to the right and to the left. Adding the two equations above, 

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/Equation3_LA.png" alt="Equation 3" class="center">
{: refdef}

In discrete form,

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/Equation4_LA.png" alt="Equation 4" class="center">
{: refdef}

For the first part that I used Green's Theorem to find the area of regular shapes. Using the skills that I learned from Activity 3, I generated a circle with radius of 0.7 units, a rectangle with length 1.4 units and width 1 unit, a tringle with height 0.5 unit of and base of 1 unit. Shown below are the synthetic images.

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/Image1_LA.png" alt="Shapes and Edges" class="center">
{: refdef}

So that Green's Theorem can be applied, the edge of the shapes above should be determined first. In Scilab, the module SIVP (Scilab Image and Video Processing toolbox) has five readily available edge detection algorithm [2]:


**sobel**
<small>Detects edges, using the sobel gradient estimator.  It isbased on convolving the image with a small, separable, and integer-valued filter in the horizontal and vertical directions [3].</small>

**prewitt**
<small>Detects edges, using the prewitt gradient estimator.  Technically, it is a discrete differentiation operator, computing an approximation of the gradient of the image intensity function [4].</small>

**log**
<small>Detects edges, using the the Laplacian of Gaussian method. sigma is the standard deviation of the LoG filter and the size of the LoG filter is nxn, where $$n = ceil(sigma*3)*2+1$$. The default value for sigma is 2.</small>

**fftderiv**
<small>Detects edges, using the FFT gradient method, default sigma 1.0</small>

**canny**
<small>Detects edges in im, using Canny method. thresh is a two-element vector, in which the fist element is the low threshold and the second one is the high threshold. If thresh is a scalar, the low threshold is $$0.4*thresh$$ and the high one is thresh. Besides, thresh can not be negative scalar. sigma is the Aperture parameter for Sobel operator, which must be 1, 3, 5 or 7. default thresh 0.2; default sigma 3.</small>

Implementing the Green's theorem and comparing the five edge detection algorithms, (the code can be found HERE [5])


{% highlight scilab %}
//Opens the image to be processed
Circle = imread('C:\Users\csrc-lab03\Desktop\yakal5.bmp');
Circle = rgb2gray(Circle)

edge_sobel = edge(Circle,'canny'); //Use edge() function sobel
imshow(edge_sobel);
imwrite(edge_sobel,'Circle_sobel.bmp');

[i_x,i_y] = find(edge_sobel); //Edge pixel coordinates

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

[theta_sorted, index] = gsort(theta','g','i');
xy_array = [x_loc;y_loc]' 
//Contains the values x and y pixels (transposed).

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

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/Image2_LA.png" alt="Actual Results" class="center">
{: refdef}

The table below shows the summary of the results of the calculated area of the shapes using Green's theorem with different edge detection algorithms. 

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/Table1_LA.png" alt="Edge Detection Algorithm Results Comparison" class="center">
{: refdef}

From visual inspection of the images, the prewitt, fftderiv and canny produced the thinnest and accurate edge detection. These visual comparisons are backed up by the low percent error of the computed area to the analytical area of the shapes. Overall, the canny algorithm seems to outperform all the other.

​

To test this, I went to google earth (left) and map (right) to searched my home, the decade old yet inviting, Yakal Residence Hall. The images below shows the sattelite view of my dorm.

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/Image3_LA.png" alt="Yakal Dormitory" class="center">
{: refdef}

I uploaded the google map image (the image on the right above) to paint, whiten the area of the building and blacken everything else. The resulting image is shown below.

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/Image4_LA.png" alt="Extracted Image" class="center">
{: refdef}

Applying Green's Theorem to find the area and comparing the five edge detection algorithms, my results are as follow.

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/Image5_LA.png" alt="Extracted Image Results" class="center">
{: refdef}

The table below shows the summary of the results of the calculated area of the shapes using Green's theorem with different edge detection algorithms. 

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/Table2_LA.png" alt="Edge Detection Algorithm Results Comparison 2" class="center">
{: refdef}

I found the actual area of the dormitory by getting the convex hull of the building in google maps. It is done by getting the smallest number of points to enclose the area. Google automotally calculated the area for me, so no sweat. The table above is fairly consistent from the results of the regular shapes. Indeed the canny algorithm best the other four. The offset of the canny method to the actual area is about 43 square meters which is roughly about four standard rooms. Not bad for my purpose of approximating areas with Green's theorem.

​

The last part of the activity is an alternative method of length and area estimation. I scanned my 6 year old DOST atm card together with a ruler (for scaling and direct measurement). I definitely look different 6 years ago. The image is shown below.

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/Image6_LA.png" alt="My DOST ID" class="center">
{: refdef}

I uploaded this image to ImageJ, a free open source software for measuring microscopic images. I drew a straight line across the horizontal and set the scales as shown below.

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/Image7_LA.png" alt="DOST in ImageJ" class="center">
{: refdef}

After setting the scales, I know that the area of my card is 45.9 sq. cm. by physical measurements. I then used the free hand selection tool to draw a polygon that fits around the sides of my card. I clicked the measurements and it gave an area of 45.842 sq. cm. which is 0.12% away from the actual value. It was accurate though I was not even making tremendous effort on making the shape really fit the sides line to line.

​

This is the best activity that I have handled so far. I felt that this topic is of practical importance. I give myself 12/10 for almost spend my whole weekend trying to diminish the error by redoing the manual graycaling in paint.

I would like to thank VISSER for the scanner, wix.com for their very nice website platform and Barts for his insights and very helpful blog.

​

​

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