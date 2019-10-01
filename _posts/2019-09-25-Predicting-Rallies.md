---
title: "Activism via Machine Learning: Modified Hidden Markov Model to forecast protest activities"
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

**Introduction**

Have you heard about Greta Thunberg? *She seems like a very happy young girl looking forward to a bright and wonderful future.* Yeah right. She is all over the news.


the young Swedish activist visibly fighting for climate justice across global summits and platforms? Most of us are attracted to her sense of purpose and advocacy.

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/rally/greta1.gif" alt="Be like Greta." class="center">
{: refdef}


**insert gif of greta with source of course**
https://gfycat.com/fancycoarsekrill-donald-trump


Have you yourself encountered an activist?

I am an activist as well. To be honest, I think being an activist in the Philippines is much harder. When you go out into the open, you will be tagged as an NPA, a communist or even a terrorist.

I am used to being red tagged. But I need to stay firm with my principles and advocacies to make impact.

In particular, when I was a student activist, my advocacy is on accessibility to education and ultimately push for free education for all. I believe that education is a right and not a privilege.

In my days in the University, I joined various demonstrations and rallies. As a science major, we've used math to calculate the feasibility of free education given the current budgeting system in the Philippines.

Eventually, through the pressure of rallies and lobbying in congress, in 2017 the Law for Free Education is passed. Now, students from various walks of life can enjoy free education in all state colleges and universities.

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/rally/test4.gif" alt="Education rallies." class="center">
{: refdef}

Education is only one part of the various struggles that Filipino citizens face from the lack of basic social services, state and government inaccountability, irresponsible mining, and the notorious extrajudicial killings a.k.a. tokhang.

These struggles are not concentrated in a certain area but is scattered across the archipelago (see image below).

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/rally/social_movement2.gif" alt="Philippine rallies." class="center">
{: refdef}

In my years as an activist, I learned that to build democracy, we need to put it in our collective hands. There are lessons in history that we should never forget like how we did it in People Power 1 and 2 against Marcos and Estrada. We can also learn lessons from recent collective actions of our neighbors in HongKong against extradition and in student rallies in Indonesia against proposed new laws on criminalization if extramarital sex and in insulting the president's honor.

In our present day, there are different forms of activism. Of particular interest for me are the so-called keyboard activists. They are the ones who initiate twitter rallies and sharing 'woke memes'.

What I am interested is when do these activist flood the streets? When will their digital words turn into analog/physical/real-life actions?

To do that I need a lot of data. Luckily, the GDELT project provides one. GDELT stands for


<blockquote>
<small>Global Database of Events, Language, and Tone (GDELT), created by Kalev Leetaru of Yahoo! and Georgetown University, along with Philip Schrodt and others, describes itself as "an initiative to construct a catalog of human societal-scale behavior and beliefs across all countries of the world, connecting every person, organization, location, count, theme, news source, and event across the planet into a single massive network that captures what's happening around the world, what its context is and who's involved, and how the world is feeling about it, every single day." [1].</small>
</blockquote>

The GDELT Project is a realtime network diagram and database of global human society for open research. [2 gdelt org website]

GDELT monitors print, broadcast, and web news media in over 100 languages from across every country in the world to keep continually updated on breaking developments anywhere on the planet. Its historical archives stretch back to January 1, 1979 and update every 15 minutes. Through its ability to leverage the world's collective news media, GDELT moves beyond the focus of the Western media towards a far more global perspective on what's happening and how the world is feeling about it.

I used Google's Big Query to get the event logs of all news in the Philippines since year 2000 (here is the script I used):

<script src="https://gist.github.com/albertyumol/3715a1cb2c5efb96269b05ac4dce0d02.js"></script>

The steps that I will follow is this:
1. Ground Set Extraction
2. Burstiness Modelling
3. Hidden Markov Modelling
4. Naive Bayes Decision

For the ground set I used GDLET. Each record has 61 fields, pertaining to a specific event in CAMEO format (What is CAMEO?).

Conflict and Mediation Event Observations (CAMEO) is a framework for coding event data (typically used for events that merit news coverage, and generally applied to the study of political news and violence).[1][2] It is a more recent alternative to the WEIS coding system developed by Charles A. McClelland and the Conflict and Peace Data Bank (COPDAB) coding system developed by Edward Azar.

I only got these fields:

SQLDATE, MonthYear, EventRootCode, GoldsteinScale, NumMentions, AvgTone, ActionGeo_CountryCode, ActionGeo_Lat, ActionGeo_Long

which I used to plot the Philippine Map gif above.

To get the ground truth, event code number 14 signifies events with mentions of PROTEST. GoldsteinScale is a numerical score ranging from -10 to 10 which signifies the theoretical potential impact that type of event will have on the stability of the country. NumMentions is the total number of mentions of this event across all source documents, which can be used as a method of assessing the importance of an event: the more the discussion of the event is, the more likely it is to be significant.

AvgTone is the average tone of all documents containing one or more mentions of this event ranging from -100 (extremely negative) to 100 (extremely positive). Action_Geo_Country code is the location of the event, ActionGeo_Lat and ActionGeo_Long are the centroid lat long of the landmark for mapping.

For the ground set extraction, I filtered out only those who have significant number of mentions across the years. I started with 2000 and aggregated on a daily basis. Plotting the time series,


{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/rally/unnormalized.png" alt="Dirty timeseries." class="center">
{: refdef}

we see that there is a heterogeneous upward trend in the event mention . To remove this, I have implemented a 90 day moving average to normalize the signal using this equation:


{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/rally/eq3.png" alt="Equation 1." class="center">
{: refdef}

To set the baseline value of number of significant event values, we define that the average mention count on each day is given by

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/rally/eq4.png" alt="Equation 2." class="center">
{: refdef}

And finally to smoothen out the data, we use a seven day moving average given by:


{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/rally/eq5.png" alt="Equation 3." class="center">
{: refdef}

where $$/theta$$ is the upper bound of the 95% confidence interval of the time series. It's a lot of math, I know but we aren't yet discussing the model yet which is more intense (and fun!).

Upon normalization, here is the result:

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/rally/normalized.png" alt="Equation 4." class="center">
{: refdef}

All of the points above the red line are days with significant rallies.

I used this points as labels to reduce my problem as supervised binary classification. Those days with significant count of rally mentions are labeled as 1 while the reverse is tagged as 0.

I will discuss the details of the modified hidden markov model that I implemented in another blog. Basically I pick Hidden Markov Model because it accounts time series variation and coupled it with Burstiness Modelling to properly account for the probabilities and duration of events.

Research in social movements point out there are events that before a big rally occur. In this study, I hypothesize that these event progression is given by this ladder:

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/rally/ladder.png" alt="Gilmore" class="center">
{: refdef}

The purpose of using hidden markov model is to approximate the likelihood of an event accuring given the probabilitiesof the progression of the states. This can be visualized by this diagram:

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/rally/states.gif" alt="Gilmore" class="center">
{: refdef}


The hidden markov model is used to estimate parameters for the model. For this, I am htraining two models. One is trained to classify if a date range contains a rally and the other model is trained to identity non-rally days as exemplified by this diagram:

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/rally/model.png" alt="Gilmore" class="center">
{: refdef}

I used 2000-2015 as my training set and 2016-2019 as my test set. I used Bayes log likelihood decision to decide which model is more accurate in a given date prediction range.

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/rally/predict.png" alt="Gilmore" class="center">
{: refdef}

For a baseline comparison of the model, since I reduced the problem into a supervised binary classification, I used Logistic regression and since it is also used in a lot of machine learning methods in the event prediction and forecasting literature. For each day, I summed over all event mentions with rootcode 14 and is it as an identifier of the event.

**Results**

The ROC curve of the models compared to a baseline is shown below:

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/rally/resulta.png" alt="Gilmore" class="center">
{: refdef}

For the accuracy and precision,

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/rally/accuracy.png" alt="Gilmore" class="center">
{: refdef}

As we can see, the modified Hidden Markov Model performed better than logistic regression. I used a time window of seven days. This mean that this particular model will be able to predict if a big rally within the next 7 days.

The results can be useful depending which side you are on. I you are an activist like me who organize people to rally for certain cause and advocacy, you would know if there is enough online clamor before a rally occur.

But if you a member of the reactionary state force, you will certainly is this prediction to suppress social movements in favor of those already in power.

My bias is that I am an activist. I believe that activism is a way of life. And we are all activists in our own little ways. Sometimes we just need to be reminder why we do the things we do and ultimately for whom do we do it.

Recommendations: Current research point out that social embeddedness, emotions, grievance and identity. This can be verified using NLP and feature importance from the news data set used above and can be done as an extension of this project.

See you in the future in future rallies and protest actions.

I will continue to join rallies and stick to my principles as I believe that only collective action can we truly achive social change. As for the corrupt politicians in the Philippines,

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/rally/greta2.gif" alt="Gilmore" class="center">
{: refdef}

https://tenor.com/view/well-be-watching-you-greta-thunberg-gif-15167876


Credits:

I want to acknowledge [Esklwelabs](https://www.eskwelabs.com/) in pursuit of this project. Also shout out to Data Science Fellow Cohort II. You are the best study buddies! I know you guys will be actuators in your future endeavors. Lots of love :)

References:

[1] Wikipedia. Global Database of Events, Language, and Tone. Retrieved from:
[https://en.wikipedia.org/wiki/Global_Database_of_Events,_Language,_and_Tone](https://en.wikipedia.org/wiki/Global_Database_of_Events,_Language,_and_Tone)

[2] The GDLET Project. Retrieved from: [https://www.gdeltproject.org/](https://www.gdeltproject.org/)



<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<script>
  (adsbygoogle = window.adsbygoogle || []).push({
    google_ad_client: "ca-pub-6410209740119334",
    enable_page_level_ads: true
  });
</script>

<div class="fb-comments" data-href="https://albertyumol.github.io/" data-numposts="5"></div>
