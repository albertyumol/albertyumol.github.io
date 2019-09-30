---
title: "DRAFT: Activism via Machine Learning: Modified Hidden Markov Model to forecast protest activities"
date: 2019-09-25
tags: [Machine Learning, Data Science]
header:
  #image: "/images/julia2.gif"
  image: "/images/rally/predicting_rallies.png"
  teaser: "/images/rally/predicting_rallies.png"
excerpt: "Social movements exhibit a complex system of social human behavior. These events demonstrate the capacity of people and their collective action to influence political decisions and public policies. This study delves into developing a model to predict future events of big rallies and protest in the Philippines by correlating it to online dissent and mentions in news outlets and social media using a coupled Burstiness and Hidden Markov Model."
mathjax: true

---
<div id="fb-root"></div>
<script async defer src="https://connect.facebook.net/en_US/sdk.js#xfbml=1&version=v3.2"></script>

Have you heard about Greta Thunberg, the young Swedish activist visibly fighting for climate justice across global summits and platforms?

**insert gif of greta with source of course**

Have you yourself encountered an activist?

I am an activist as well. To be honest, I think being an activist in the Philippines is much harder. When you go out into the open, you will be tagged as an NPA, a communist or even a terrorist.

I am used to being red tagged. But I need to stay firm with my principles and advocacies to make impact.

In particular, when I was a student activist, my advocacy is on accessibility to education and ultimately push for free education for all. I believe that education is a right and not a previledge.

In my days in the University, I joined various demonstrations and rallies. As a science major, we've used math to calculate the feasibilty of free education given the current budgeting system in the Philippines.

Eventually, through the pressure of rallies and lobbying in congress, in 2017 the Law for Free Education is passed. Now, students from various walks of life can enjoy free education in all state colleges and universities.

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/rally/test4.gif" alt="Gilmore" class="center">
{: refdef}

Education is only one part of the various struggles that Filipino citizens face from the lack of basic social services, state and government inaccountability, irresponsible mining, and the notorious extrajudicial killings a.k.a. tokhang.

These struggles are not concentrated in a certain area but is scattered across the archipelago (see image below).

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/rally/social_movement2.gif" alt="Gilmore" class="center">
{: refdef}

In my years as an activist, I learned that to build democracy, we need to put it in our collective hands. There are lessons in history that we should never forget like how we did it in People Power 1 and 2 against Marcos and Estrada. We can also learn lessons from recent collective actions of our neighbors in HongKong against extradition and in student rallies in Indonesia against proposed new laws on criminalization if extramarital sex and in insulting the president's honor.

In our present day, there are different forms of activism. Of particular interest for me are the so-called keyboard activists. They are the ones who initiate twitter rallies and sharing 'woke memes'.

What I am interested is when do these activist flood the streets? When will their digital words turn into analog/physical actions?

To do that I need a lot of data. Luckily, the GDELT project provides one. GDELT stands for


<blockquote>
<small>Global Database of Events, Language, and Tone (GDELT), created by Kalev Leetaru of Yahoo! and Georgetown University, along with Philip Schrodt and others, describes itself as "an initiative to construct a catalog of human societal-scale behavior and beliefs across all countries of the world, connecting every person, organization, location, count, theme, news source, and event across the planet into a single massive network that captures what's happening around the world, what its context is and who's involved, and how the world is feeling about it, every single day." [1].</small>
</blockquote>

The GDELT Project is a realtime network diagram and database of global human society for open research. [2 gdelt org website]

GDELT monitors print, broadcast, and web news media in over 100 languages from across every country in the world to keep continually updated on breaking developments anywhere on the planet. Its historical archives stretch back to January 1, 1979 and update every 15 minutes. Through its ability to leverage the world's collective news media, GDELT moves beyond the focus of the Western media towards a far more global perspective on what's happening and how the world is feeling about it.

I used Google's Big Query to get the event logs of all news in the Philippines since year 2000 (here is the script I used):

<script src="https://gist.github.com/albertyumol/3715a1cb2c5efb96269b05ac4dce0d02.js"></script>


I will be posting my code in Github soon (standby).









References:

[1] Wikipedia. Global Database of Events, Language, and Tone. Retrieved from:
[https://en.wikipedia.org/wiki/Global_Database_of_Events,_Language,_and_Tone](https://en.wikipedia.org/wiki/Global_Database_of_Events,_Language,_and_Tone)

[2] The GDLET Project. Retrieved from: [https://www.gdeltproject.org/](https://www.gdeltproject.org/)


[1] M. Soriano, “Length and Area estimation in images,” Applied Physics 186 Activity Hand-outs, 2014.

[2] Scilab Image Processing. Retrieved from:
[http://siptoolbox.sourceforge.net/doc/sip-0.7.0-reference/edge.html](http://siptoolbox.sourceforge.net/doc/sip-0.7.0-reference/edge.html)

[4] Wikipedia. Prewitt Operator. Retrieved from:
[http://siptoolbox.sourceforge.net/doc/sip-0.7.0-reference/edge.html](https://en.wikipedia.org/wiki/Prewitt_operator)

[5] Barteezy's Applied Physics Experience. Retrived from: [http://siptoolbox.sourceforge.net/doc/sip-0.7.0-reference/edge.html](https://barteezy.wordpress.com/2015/09/06/activity-4-length-and-area-estimation-in-images/)



<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<script>
  (adsbygoogle = window.adsbygoogle || []).push({
    google_ad_client: "ca-pub-6410209740119334",
    enable_page_level_ads: true
  });
</script>

<div class="fb-comments" data-href="https://albertyumol.github.io/" data-numposts="5"></div>
