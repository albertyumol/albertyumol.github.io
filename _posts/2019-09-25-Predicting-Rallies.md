---
title: "Activism via Machine Learning: Modified Hidden Markov Model to forecast protest activities"
date: 2019-09-25
tags: [Machine Learning, Data Science]
header:
  image: "/images/rally/title.png"
  teaser: "/images/rally/front.png"
excerpt: "Social movements exhibit a complex system of social human behavior. These events demonstrate the capacity of people and their collective action to influence political decisions and public policies. This study delves into developing a model to predict future events of big rallies and protest in the Philippines by correlating it to online dissent and mentions in news outlets and social media using a coupled Burstiness and Hidden Markov Model."
mathjax: true

---
<div id="fb-root"></div>
<script async defer src="https://connect.facebook.net/en_US/sdk.js#xfbml=1&version=v3.2"></script>

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/rally/front.png" alt="me." class="center">
{: refdef}

Have you heard about Greta Thunberg? That *'...very happy young girl looking forward to a bright and wonderful future'.* She is all over the news and my twitter feed recently. Most of those in my digital circle post a lot of her *gifs* as she voice out her advocacies on the main stage of international climate summits. A lot seems to be attracted by her sense of purpose and 'woke-ness' at a young age.



There seems to be a lot of online clamor showing support for her activism. We tend to be drawn to hero figures like her and the extra-ordinariness of her cause. But sometimes, actions need not to be very extra-ordinary to be heroic.

Let me ask you if you yourself have met an activist like her? It seems unlikely when you live in the Philippines.

I myself is an activist like Greta. I may not be as young as her but I am just as pretty :). To be honest, I think being an activist in the Philippines (regardless of advocacy e.g. environment, basic social services, human rights, etc.) is much harder and dangerous. If you declare yourself as an activist and go out into the open, often than not you will be tagged as a member of the New Peoples Army, a communist, or even a radical terrorist. Being an activist in the Philippines means acceptance of the perils and circumstances that comes with it.

If you are active on Facebook, chance is high that you might have encountered news about atrocities against activists (mainstream media don't usually report it). Just recently, news broke about forest rangers and protectors in Palawan being killed by paramilitary men and illegal loggers. I think hard about this. **How can we tell the future generations to love and protect nature when people who do that suffer and are killed?**

As for my case, I have accepted these consequences and stay firm with my principles and advocacies to make a difference. When I was a student activist in University, my advocacy is on accessibility to education and ultimately pushing for free education for all. I believe that education is a right and not a privilege. I joined various demonstrations and rallies to forward and lobby this. As a science major, I always integrate my math skills to calculate the feasibility figures of free education and put it creatively in our rally boards and chants. The streets became my laboratory and the struggles of poor students became my thesis.

Eventually, through the pressure of decade long big rallies (see image below) and actual lobbying in congress (yes), in August 2017 The Universal Access to Quality Tertiary Education Act, or Republic Act 10931 was signed into law. Now, students from various walks of life can enjoy free education in all state colleges and universities in the country.

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/rally/test4.gif" alt="Education rallies." class="center">
{: refdef}

Education is only one part of the various struggles that Filipino citizens face on a daily basis spanning from the lack of basic social services, government unaccountability and state neglect, irresponsible mining, and the notorious extrajudicial killings in the face of *tokhang*. These struggles happen across the archipelago and experienced by various sectors of our society.

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/rally/social_movement2.gif" alt="Philippine rallies." class="center">
{: refdef}

In my years as a student activist, I learned that to build democracy, we need to put it in our collective hands. There are lessons in history that we should never forget like how we did it in People Power 1 and People Power 2 against the dictator Marcos and jueteng-lord Estrada. We can also learn lessons from recent collective actions of our neighbors in Hong Kong against extradition and in student rallies in Indonesia against proposed new laws on criminalization if extramarital sex and in insulting their president's honor.

Nowadays, much of our time are spent online, thus emerges a new type of activists. They are often called as keyboard activists. They are the ones who initiate twitter rallies and share 'woke memes'. Maybe you yourself have shared or retweeted their content.

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/rally/twitter.jpg" alt="Twitter rallies." class="center">
{: refdef}

For this particular project, what I am interested is **when do these activists flood the streets? When will their digital words turn into real-life actions?**

**Data Wrangling**

To do that I need a lot of data. Luckily, the GDELT project provides more than enough.

<blockquote>
<small>Global Database of Events, Language, and Tone (GDELT), created by Kalev Leetaru of Yahoo! and Georgetown University, along with Philip Schrodt and others, describes itself as "an initiative to construct a catalog of human societal-scale behavior and beliefs across all countries of the world, connecting every person, organization, location, count, theme, news source, and event across the planet into a single massive network that captures what's happening around the world, what its context is and who's involved, and how the world is feeling about it, every single day." [2].</small>
</blockquote>

The GDELT Project is a real time network diagram and database of global human society for open research [3]. It monitors print, broadcast, and web news media in over 100 languages from across every country in the world to keep continually updated on breaking developments anywhere on the planet. Its historical archives stretch back to January 1, 1979 and update every 15 minutes. Through its ability to leverage the world's collective news media, GDELT moves beyond the focus of the Western media towards a far more global perspective on what's happening and how the world is feeling about it.

I used Google's Big Query to get the event logs of all news in the Philippines since year 2000 (here is the script I used):

<script src="https://gist.github.com/albertyumol/3715a1cb2c5efb96269b05ac4dce0d02.js"></script>

These are the steps that I followed:

<blockquote>
<small>
1. Ground Set Extraction <br>
2. Burstiness Modelling <br>
3. Hidden Markov Modelling <br>
4. Naive Bayes Decision
</small>
</blockquote>

Each record in GDELT has 61 fields, pertaining to a specific event in CAMEO format.

<blockquote>
<small>
Conflict and Mediation Event Observations (CAMEO) is a framework for coding event data (typically used for events that merit news coverage, and generally applied to the study of political news and violence). [4]
</small>
</blockquote>

For my daily aggregation, I only need and obtained these fields:

<blockquote>
<small>
SQLDATE, MonthYear, EventRootCode, GoldsteinScale, NumMentions, AvgTone, ActionGeo_CountryCode, ActionGeo_Lat, ActionGeo_Long
</small>
</blockquote>

Note:
<blockquote>
<small>
<b>GoldsteinScale</b> is a numerical score ranging from -10 to 10 which signifies the theoretical potential impact that type of event will have on the stability of the country. <b>NumMentions</b> is the total number of mentions of this event across all source documents, which can be used as a method of assessing the importance of an event: the more the discussion of the event is, the more likely it is to be significant. <b>AvgTone</b> is the average tone of all documents containing one or more mentions of this event ranging from -100 (extremely negative) to 100 (extremely positive) [5]. <b>Action_Geo_Country</b> code is the location of the event, <b>ActionGeo_Lat</b> and <b>ActionGeo_Long</b> are the centroid latitude and longitude of the landmark which I used to plot the Philippine map gif above.
</small>
</blockquote>


**Ground Set Extraction**

The events in GDELT are categorized by identified themes labeled 1 to 20. The theme includes verbs describing the type of action of an event like reject (12), protest (14), threaten (13), coerce (17), assault (18), etc.

To get the ground truth, event root code number 14 signifies events with mentions of PROTEST. I filtered out only those who have significant number of mentions across the years. I started with 2000 and aggregated on a daily basis. Plotting the time series,

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/rally/unnormalized.png" alt="Dirty timeseries." class="center">
{: refdef}

we see that there is a heterogeneous upward trend in the event mentions [5]. To remove this, I have implemented a 90 day moving average to normalize the signal using this equation:

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/rally/eq3.png" alt="Equation 1." class="center">
{: refdef}

To set the baseline value of number of significant event values, we define that the average mention count on each day is given by

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/rally/eq4.png" alt="Equation 2." class="center">
{: refdef}

and to smoothen out the data, we use a **seven day** moving average:

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/rally/eq5.png" alt="Equation 3." class="center">
{: refdef}

where $$\theta$$ is the upper bound of the 95% confidence interval of the time series. It's a lot of math, I know (same girl I can relate, also took me a while to understand this) but we aren't yet discussing the model which is more intense (and fun!).

Upon normalization, here is the result:

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/rally/normalized.png" alt="Equation 4." class="center">
{: refdef}

All of the points above the red line are days with significant rallies.

I used this points as labels and reduce my problem to a supervised binary classification. Those days with significant count of rally mentions are labeled as 1 while those with none are tagged as 0.

I will discuss the details of the coupled Burstiness and Hidden Markov Model (HMM) that I implemented in another blog post. I chose HMM because it accounts for time series variation as sequence learning and coupled it with Burstiness Modelling to properly account for the probabilities and duration of events (also called as states).

Research in social movements point out there are mini-events that lead to the occurrence of a big rally. In this study, I hypothesize that this event progression is given by this ladder:

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/rally/ladder.png" alt="Ladder of Social Movement" class="center">
{: refdef}

These five so-called states (identified in GDLET as event root codes 10, 11, 12, 13, and 14) are what I used to define the observation vector needed by the HMM. To increase accuracy of prediction, I also added the AvgTone and GoldsteinScale (discussed above) to the dimension of the observation. Thus, the final vector is given by:

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/rally/eq2.png" alt="Equation 4." class="center">
{: refdef}

HMM is a strong candidate for this problem because it can approximate the likelihood of an event occurring given the probabilities of the progression of the states. This statement can be visualized by this diagram:

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/rally/states.gif" alt="Probability States" class="center">
{: refdef}

The numbers in the diagram indicates probabilities when a particular state will transition to another state. HMM is used to estimate the parameters for the model. To increase the accuracy of the solution, I trained two models. One is trained to classify if a date range contains a rally (SM-prone) and the other model is trained to identity non-rally days (SM-free) as exemplified by this diagram:

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/rally/model.png" alt="Modelling diagram" class="center">
{: refdef}

I used **2000-2015 as my training set** and **2016-2019 as my test set**. I implemented a Bayes log likelihood decision mechanism to decide which model is more accurate in a given date prediction range.

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/rally/predict.png" alt="Prediction scheme" class="center">
{: refdef}

For baseline model comparison, since I reduced the problem into a supervised binary classification, I used *Logistic Regression*. It is commonly used in a lot of machine learning methods in the event prediction and forecasting literature. For each day, I summed over all event mentions with event root code 14 and use it as an indicator of big rallies.

**Results**

The ROC curve of the model compared to the *Logistic Regression* baseline is shown below:

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/rally/resulta.png" alt="ROC curves" class="center">
{: refdef}

Here are the accuracies and precisions:

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/rally/accuracy.png" alt="Performance metrics" class="center">
{: refdef}

As we can see, the modified Hidden Markov Model performed better than Logistic Regression. Using a time window of seven days means that this particular model implementation will be able to predict if a big rally will happen within the next 7 days.

**Conclusion**

The results can be useful depending on your biases. If you are an activist like me who organize people to rally for certain causes and advocacies, you would know if there is enough online clamor before a rally occur and encourage more discussions to meet a certain threshold value.

If you are a member of the reactionary state force, you will most likely use this prediction to suppress social movements in favor of the status quo.

If you are a normal citizen, you decide whether the issues being talked about speak relevance and weigh in future consequences.

**Recommendations**

Current research point out that social embeddedness, emotions, grievance and identity [6] are the most important features that influence protest activities. This can be verified using NLP and feature importance from the news data set used above and can be done as an extension of this project.

**Takeaways**

My bias is that I am an activist. I believe that activism is a way of life. And everyone of us are all activists in our own little ways. Sometimes we just need to be reminded why we do the things we do and ultimately ask *for whom* do we do these things.

For my case, I will continue to join rallies and stick to my principles as I believe that only through collective action can we truly achieve genuine social change. As for the corrupt politicians in the Philippines,

{:refdef: style="text-align: center;"}
<img src="{{ site.url }}{{ site.baseurl }}/images/rally/greta2.gif" alt="Greta is angry." class="center">
{: refdef}


See you in the future and hopefully in future rallies and protest actions.
#BeLikeGreta
#DataScienceForThePeople



**Credits**

I want to acknowledge [Esklwelabs](https://www.eskwelabs.com/) in pursuit of this project. Also shout out to Data Science Fellow Cohort II. You are the best data science study buddies! I know you guys will be actuators in your future endeavors. Sending lots of love and virtual hugs :)

Want to collaborate? Message me in [LinkedIn](https://ph.linkedin.com/in/albertyumol).

**References**

[1] Gfycal. Donald Trump and Greta Thunberg 1. Retrieved from :
[https://gfycat.com/fancycoarsekrill-donald-trump](https://gfycat.com/fancycoarsekrill-donald-trump)

[2] Wikipedia. Global Database of Events, Language, and Tone. Retrieved from:
[https://en.wikipedia.org/wiki/Global_Database_of_Events,_Language,_and_Tone](https://en.wikipedia.org/wiki/Global_Database_of_Events,_Language,_and_Tone)

[3] The GDLET Project. Retrieved from: [https://www.gdeltproject.org/](https://www.gdeltproject.org/)

[4] Wikipedia. Conflict and Mediation Event Observations. Retrieved from: [https://en.wikipedia.org/wiki/Conflict_and_Mediation_Event_Observations](https://en.wikipedia.org/wiki/Conflict_and_Mediation_Event_Observations)

[5] Discrete Dynamics in Nature and Society. Predicting Social Unrest Events with Hidden Markov Models Using GDELT. Retrieved from: [https://www.hindawi.com/journals/ddns/2017/8180272/](https://www.hindawi.com/journals/ddns/2017/8180272/)

[6] EPJ Data Science. Activism via attention: interpretable spatiotemporal learning to forecast protest activities. Retrieved from: [https://epjdatascience.springeropen.com/articles/10.1140/epjds/s13688-019-0183-y](https://epjdatascience.springeropen.com/articles/10.1140/epjds/s13688-019-0183-y)

[7] Gfycal. Donald Trump and Greta Thunberg 2. Retrieved from :
[https://tenor.com/view/well-be-watching-you-greta-thunberg-gif-15167876](https://tenor.com/view/well-be-watching-you-greta-thunberg-gif-15167876)


<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<script>
  (adsbygoogle = window.adsbygoogle || []).push({
    google_ad_client: "ca-pub-6410209740119334",
    enable_page_level_ads: true
  });
</script>

<div class="fb-comments" data-href="https://albertyumol.github.io/" data-numposts="5"></div>
