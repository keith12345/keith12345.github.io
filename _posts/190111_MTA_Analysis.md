---
layout: post
title: Determining Ideal Busking Locations in NYC Subways based on MTA Data
---


<div class="message">
  For this project I looked at exit and entry information from the MTA.info website and cross-referenced that with lists of popular and allowed busking locations to determine ideal busking locations, i.e. those at the intersection of low competition and high traffic.
</div>

[Busking](https://en.wikipedia.org/wiki/Street_performance), or street performance, can be an important source of income for musicians, especially those in urban areas. New York City has a program called [MUNY](http://web.mta.info/mta/aft/muny/) (Music Under New York) in which they partner with NYC musicians and allow them to schedule performances in designated subways. The program can be quite competitive, requiring auditions to acquire a permit.

## Gathering Outside Data:
In addition to the provided MTA.info dataset I gathered outside sources to increase the quality of the recommendations. For the list of participating stations I used those listed on MTA.info website under Arts and Design. That list was then cross-referenced with lists of *sought after* locations. Note that 'sought after', used synonymously with competitive, is used somewhat subjectively here and is based on recommended spots to see busking in NYC which were gathered from various blogs; the links to which can be found below.

## Choosing the Stations:
While performing in front of a large number of people is a priority, there are other factors to consider such as the number of transfers. The number of riders within a station with transfers is likely to be higher, perhaps even significantly so, than is indicated by entries and exits. It is also possible that riders spend significantly more time in stations with transfers. Given more time, these variables would have been weighed much more heavily in determining ideal locations.

Only stations within those listed on the MTA Arts and Design site were considered in the recommendations. Sought after stations were excluded from the results as there is typically a long waiting list to perform and the hope was to provide immediately actionable results.

## Timeframe:
While busking takes place in subways throughout the year, there are benefits and drawbacks to every season. Subway stations are generally [very hot and humid](https://www.citylab.com/transportation/2018/08/its-way-too-hot-on-the-new-york-city-subway/567278/) during the summer, much of Q2 and Q3. Heat is known to have [considerable impact](https://journals.sagepub.com/doi/abs/10.1177/0146167295215002) on one's mood which could to translate less generosity. Likewise, subways are very cold during winter, especially Q1. That, coupled with the reduced travel and expended holiday spirit, are likely not conducise to the highest level of success.

Ultimately, I decided to focus on Q4 for recommendations; capitalizing on the somewhat temperate climate in the subways, the influx of tourists, and generousity and holiday spirit of riders.

## Visualizing our results:
Upon visualizing results a surprising finding was that the volume of entries and exits at sought after stations was not much different from the average of all participating stations.
![Average_Hourly_Volume](/images/Average_Hourly_Volume.png "Average_Hourly_Volume")



Within the list of stations 

## Links and Sources