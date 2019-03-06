---
layout: post
title: Job Hunting with NLP
---


<div class="message">
My goal in approaching this project was to use Natural Language processing to better understand what different companies are looking to facilitate my job search.
</div>

## Problem Statement
One of the more frequent topics of conversation with the other fellows at Metis is navigating the ambiguous titles and job descriptions in data-related fields. Some companies may define a particular role as a data scientist while others see it as a machine learning engineer. Others still may feel that the role is within the product team and is some form of a product analyst.  

In short; how do we find the jobs for which we are most qualified, not based on the job title but on the requirements of the position.


## Gathering Data
For this project, I sourced the data from indeed.com using selenium and Beautiful Soup 4. I focused only on jobs in data-related fields. I searched on location (5 total) and keyword (7 total) combinations resulting in 35 searches. However, for the recommender, I used only results from the San Francisco Bay Area.  

For each job I collected the job title, company name, bulleted items within the job description, paragraph items within the job description, salary, and the URL so that I could access the jobs later if I wanted.

#### Key Words:
* **Data Scientist**
* **Machine Learning Engineer**
* **Data Engineer**
* **Research Engineer**
* **Artificial Intelligence Engineer**
* **Product Analyst**
* **Product Scientist**

#### Locations:
* **San Francisco Bay Area**
* **New York**
* **Seattle**
* **Los Angeles**
* **Chicago**

## Data prep
### N-grams
I wasn't thrilled with the lack of visibility and flexibility offered for n-gram creation within gensim, so I decided to create my a new tool. With it, I was able to easily see which n-grams were occurring and the frequency at which they occurred. I was then able to have more precise cut-offs for the number of occurrences required for particular n-grams to make it into my model. e.g., I was then able to see where to have my cutoff so that I included relevant terms like _spark_hadoop_ and excluded meaningless word co-occurrences such as a _team_ensure_.

### Stopwords
An interesting issue that I encountered during this project became particularly apparent during topic modeling, the presence of terms describing 'perks' which had almost an entire topic dedicated to it.

While some people may want to search for a job based on its perks, that was not the intended result of this project. Consequently, I removed words such as medical, dental, vision, 401k and vacation, so the model would match on the job alone.  

## Word Embedding - Making my job descriptions 'Computer Readable'

### Which text to use
Upon inspection of some of the job descriptions, I noticed that the paragraph sections within each job description didn't always contain information specific to the role; it was typically general information about the company:  


![Job_Description_Paragraph](/images/example_paragraph_item.png "An example paragraph item taken from a job description showing that the description is more about the company, its culture, and its product, rather than about the position in question.")  


As a result, I decided to use only the listed items for each job description which you can see are more tailored to the specific role than to the company:  


![Job_Description_listed_items](/images/example_listed_items.png "An example group of listed items taken from a job description showing that the listed items are more about tailored to the role than the company.")  


Note that the perks mentioned above were also typically included as listed items and were thus not excluded by not using paragraph sections.

### Testing CountVectorizer and TF-IDF with NMF*
While there are numerous word embedders available, not all work on limited data. I only had a few thousand job descriptions, and models like Word2vec of GloVe work best when given terabytes, not megabytes, of data. I experimented with the pre-trained versions of those models but, as they were not trained on data similar to my own (i.e., job descriptions), topic modeling with the word vectors created by them did not group things like cloud and azure into the same topic. This is likely because Word2vec and GloVe were thinking of 'cloud' in the _meteoroligical_ sense, not in terms of _computing_, as that is how it was used in the data on which they were trained.

That said, both CountVectorizer and TF-IDF work well with smaller amounts of data. CountVectorizer had trouble with my dataset, indicating 'etc' as a significant word in topic modeling during early testing. TF-IDF, on the other hand, performed remarkably well. I, therefore, did not use CountVectorizer going forward in other topic modeling tests. 

Once deciding on TF-IDF as my word embedder I plotted the inertia resulting from various numbers of means in KMeans clustering. Unlike when I performed topic modeling with the word vectors created by Word2vec, the topic modeling with the vectors created by TF-IDF was able to recognize that 'cloud' and 'azure' were within the same topic. I also created the following topics using NMF with TF-IDF (topic names are my own):

#### Business Analysis
* **analytics**
* **insight**
* **sql**
* **business**
* **analysis**

#### Business Analysis
* **tensorflow**
* **model**
* **ml**
* **algorithm**
* **production**

Before deciding to use 9 topics while topic modeling I had plotted the inertia resulting from using KMeans clustering to get an idea of how many topics there might be in my dataset. See below:


![KMeans_inertia](/images/KMeans_inertia.png "A line plot of the inertia resulting from various numbers of means in KMeans clustering highlighting elbows and the actual number of topics used.")


I noted elbows at 11, 15, 18, 21, and 23 means (in green) and performed topic modeling for each of those numbers of means. Interestingly, the ideal number of topics was not the same as the ideal number of means; redundancies occurred with larger numbers of topics. This is likely a result of the initial problem we students at Metis have been discussing all along; the fact that not all data-related roles fit nicely into a single silo!

*Explaining the above mentioned models and terms is not the purpose of this post. For further information wikipedia has great articles on the following subjects: [TF-IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf), [CountVectorizer](https://en.wikipedia.org/wiki/Bag-of-words_model), [Word2vec](https://en.wikipedia.org/wiki/Word2vec), [GloVe](https://en.wikipedia.org/wiki/GloVe_(machine_learning)), [NMF](https://en.wikipedia.org/wiki/Non-negative_matrix_factorization), [KMeans](https://en.wikipedia.org/wiki/K-means_clustering), [PCA](https://en.wikipedia.org/wiki/Principal_component_analysis).

## Visualizing our results
Below you can see an interactive plot using plotly reduced to 3 dimensions using PCA. For each plot you can see the following information:  

* Company Name
* Predicted Role
* Actual Title
* Partition Size
* URL

<div>
    <a href="https://plot.ly/~keith12345/156/?share_key=2T8MkDmrQoPmuWxYb0IpoZ"
    target="_blank"
    title="plot from API (23)"
    style="display: block; text-align: center;">
    <img src="https://plot.ly/~keith12345/156.png?share_key=2T8MkDmrQoPmuWxYb0IpoZ"
    alt="plot from API (23)"
    style="max-width: 100%;width: 600px;"
    width="600"
    onerror="this.onerror=null;this.src='https://plot.ly/404.png';" />
    </a>
  <script data-plotly="keith12345:156"
    sharekey-plotly="2T8MkDmrQoPmuWxYb0IpoZ"
    src="https://plot.ly/embed.js"
    async>
  </script>
</div>

Note that at the outer limits of the plot the roles are more defined while toward the middle of the plot there is some overlap in the roles. This could result from inaccuracy in the model, but it is also possible that an employer gave a particular title to a role that is not most applicable when compared to other positions.

## How can we use our results

While these insights are interesting, they're not useful on their own. However, back to the issue we described earlier, what if there was a way to use our new insights to search for a job in a way other than merely by employer, title, or keyword?  

My solution to this issue was to create a web application using JavaScript and Python in which a user can input their ideal job description and see similar roles. Roles are matched based on the cosine similarity of the PCA of the vectorization of the entered job description. You can see that as the user changes the job description and combines different descriptions, different roles appear.

<iframe width="781" height="556" src="https://www.youtube.com/embed/V1LpwNl6WZw" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

While I have not yet launched the app, I plan on launching a version that will be available to the public and will update regularly with new postings. 

## GitHub Repo
[Job Hunting with NLP](https://github.com/keith12345/Find_me_a_Job/)