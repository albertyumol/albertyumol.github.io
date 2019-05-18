---
title: "when you need a laptop but you broke: chronicles of a tech activist in 3rd world"
date: 2019-05-18
tags: [open Hardware]
header:
  #image: "/images/julia2.gif"
  #image: "/images/backdrop7.png"
  teaser: "/images/open_hardware/thinkpad.jpg"
excerpt: "It's time to find a new laptop. But I have one major problem. I'm broke. As an un-salaried NGO volunteer, I have very limited budget from my savings. At max, I can only spend around 10,000 pesos - all in. As a thrift-techie enthusiast that I am, I happen to be familiar with the nooks and crannies of the cheapest finds for most things that run with electricity. During my spare time, I just usually visit these thrift-tech stores just looking around, familiarizing myself with latest gadgets (not necessarily phones and laptops)."
mathjax: true

---
<div id="fb-root"></div>
<script async defer src="https://connect.facebook.net/en_US/sdk.js#xfbml=1&version=v3.2"></script>

Welcome to my first entry on open electronics!It has been a while since my last blog entry. You know, as an activist, the political arena in the Philippines takes a lot on people. Who won't be stressed with Duterte's
Drug war, threats of Martial Law, Jeepney phaseout, the unsolicited Build Build Build Program and its Tax Reform for Acceleration and Inclusion (TRAIn) Law? Let's take a short break from all of these commotions and discuss how I managed to switch to a new laptop on a budget.
​
I have been a MacBook Pro user for about 5 years already. One of my weird practice is to christen my computers. I named my MacBook Merlin based from the fictional Welsh figure from the legend of King Arthur because I really like magick personalities. Merlin served me well since he's a Mac and I use computational power intensively, like running python codes for weeks and video editing on the side.

But even before, I have had software and hardware compatibility issues with Mac. It is not straightforward upgrading Java or even running Arduino IDE. Often I used virtual machines to run my favorite Linux distros which of course by itself is very memory intensive. Added to these issues are of security and weight. Recently I have had Ads popping up with my browser closed. Mac is also very heavy and heats up a lot! All in all, my Mac experience has been good in that it can sustain my daily dose of computational power but it felt very restricting in terms of the amount of modification I can render to the system.
​
Time came when I finally gave up on Mac and wiped it clean with a new Ubuntu OS. But of course Linux on Mac is not necessarily a good idea because the MacBook is made for MacOS.

So I decided to move on and charge it to my experience. It's time to find a new laptop. But I have one major problem. I'm broke. As an un-salaried NGO volunteer, I have very limited budget from my savings. At max, I can only spend around 10,000 pesos - all in.

As a thrift-techie enthusiast that I am, I happen to be familiar with the nooks and crannies of the cheapest finds for most things that run with electricity. During my spare time, I just usually visit these thrift-tech stores just looking around, familiarizing myself with latest gadgets (not necessarily phones and laptops).

My top choice is the well-known tech-thrift place in Manila - Gilmore I.T. Center.

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/open_hardware/gilmore.png" alt="Gilmore" class="center">
{: refdef}

This place is a haven for people like me. Everything is cheap and most of the prices can be haggled. Before, I went to Gilmore, I did initial research and created a checklist for the cheap laptop. As a self-proclaimed hipster, form factor is important. I want it to be light and unique. My research brought me to a specific brand that suited my criteria - the IBM Thinkpad.

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/open_hardware/lenovo.png" alt="Lenovo X200" class="center">
{: refdef}

Just look at that monster! How hipster could you get with that legendary mechanical keyboard and the red trackpoint as a mouse? At first sight, I was really flabbergasted by the design of the IBM Lenovo Thinkpad. It came to a point that this monster visited me in sleep.​​

The next morning, I found myself gallivanting around Gilmore IT Center looking for my next best friend. I found many blogs and vlogs on a particular model of the Lenovo Thinkpad but one rose above them all - the mighty x200.

I was already at the 4th floor of the building looking keenly on my catch. I found a lot of good stalls with really crazy offers such as PC Green.

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/open_hardware/pc_green.jpg" alt="PC Green" class="center">
{: refdef}

They sell laptop units for as low as 2500 pesos for brands which includes NEC, Fujitsu, Samsung - mostly Japanese surplus. Sadly, they don't have the x200. As I was about to lose hope that day, I turned to the last shop in front of PC Green -  Rhem's Computer Trading.

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/open_hardware/rem.png" alt="REM Shop" class="center">
{: refdef}

A sales lady approached me and asked what I was looking for. I said Thinkpad x200 and she replied knowingly pitching about hardware specs and availability. Luckily, they have one more stock of the model.

And lo and behold, I was on my way home ready to tweak my loot. The unit that I bought was almost good as new (99% smooth) with 2GB DDR3 of RAM, Intel Core 2 Duo, 160 GB of HDD for a measly 5000 pesos.

Before I went home, I also bought a 120 GB SSD drive to ramp up the computer speed. The first thing that I did upon returning home was replacing the drive and adding another 2 GB DDR3 of RAM. Here are photos of the WD SSD I bought for 2900 pesos and my spare RAM from my previous laptop.

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/open_hardware/internal.png" alt="Internal parts" class="center">
{: refdef}

The laptop came with Windows 7 Vista installed but as a Linux boy since high school, Windows was never and will never be good enough for me. At first, what I planned to do was to install an open bios called Libreboot to the laptop but I am currently in no luck to find a SOIC IC connector pin for me to hack the hardware. This is what the SOIC pin looks like, so if you happen to have one please send me help.

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/open_hardware/soic.png" alt="SOIC Jumper" class="center">
{: refdef}

There is a work around however but it requires me soldering wires through the motherboard and connecting it to GPIO pins of a Raspberry Pi. I'll probably do that as a last option. Why am I so concerned with changing the BIOS loader? Because Intel processors are rumored to have backdoors. For fool proof security, this needs to be hacked. For now, I'll settle for a plain Linux installation. Here is what the laptop looks like.

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/open_hardware/finished.png" alt="Actual Pretty Laptop" class="center">
{: refdef}

I installed my favorite distro, the gorgeous Elementary OS. My only concern now is the battery life which is not really bad. It clocks at around 90 minutes so when I have the funds I will buy a 9 cell battery for a day long of volt juice.

I have also made my ceremonies upon the installation of the OS. I have installed my security basics and networking such as TOR browser, VeraCrypt, and the nmap packages. I have also ramped up the desktop experience by installing numix. Here is a snapshot of my desktop.

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/open_hardware/desktop.jpg" alt="Desktop" class="center">
{: refdef}

Gorgeous isn't it? Amazingly, the x200 line offers one of the very first finger print scanning for logging in. I was able to install it in a jiffy by using the package FPrint with this code.

<blockquote>
<small>sudo apt install libpam-fprintd fprint-demo</small>
</blockquote>

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/open_hardware/fingerprint.jpg" alt="Fingerprint" class="center">
{: refdef}

Booting takes at most 20 seconds. The experience felt amazing. My applications that took long in Mac to load, run in an instant. Thanks to the SSD and the power of Linux!​​

In summary, I was able to find a laptop on budget! 'Maxed' the specs up still on a budget and run it with free and open source softwares. Looking forward, I hope to find a SOIC connector soon so I can Libreboot my laptop like the Unix God Richard Stallman.

But so far, I am a very happy kid with this find. I have sought help from my geek friends to find an oracle that can lead me to a SOIC connector. With that I'm gonna name the laptop Endor, after the sorceress who King Saul of the bible sought to predict his fate with the war against Philistines.

What do you think? Leave a comment below, and don't forget to say hi.

<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<script>
  (adsbygoogle = window.adsbygoogle || []).push({
    google_ad_client: "ca-pub-6410209740119334",
    enable_page_level_ads: true
  });
</script>

<div class="fb-comments" data-href="https://albertyumol.github.io/" data-numposts="5"></div>
