![readme_cover](https://github.com/leo-schell/IntentionalDiversity_RecSystems/assets/122314061/230bef90-8cb0-414f-9ee6-78a0354634cf)

By Leo Schell Villanueva

# Overview
This project is a study in promoting different kinds of diversity within simple recommendation algorithms on content based platforms for streaming and social media.

If you are chronically online like I am, you've probably noticed that social media algorithms tend to niche down really quickly. For example, I don't know how I ended up on Scottish Sheep Farmer TikTok, but it is an absolute delight. My favorite farmer, [@seanthesheepman](https://www.tiktok.com/@seanthesheepman?lang=en), posts weekly updates of his dog Katie and through his content my world has become a little richer.

However, there was a period of time where the vast majority of content recommended to me was about Scottish sheep. No other sheep, just Scottish sheep. Moments like this can be frustrating for users. **From racial diversity to content diversity, we know that all content-driven platforms can do better.**

As data professionals, we participate in this conversation in the form of building infrastructure that allows users to access interesting and novel content. In this project, I examine simple recommendation algorithms that are similar to those used by Netflix built using [scikit Surprise](https://surpriselib.com/) for racial and content diversity. I then use those results to construct an easy solution to the major problems these algorithms face.

**Table of Contents**
- [Background](#background)
- [Data Understanding](#data-understanding)
- [Models Analyzed](#models-analyzed)
- [Final Recommender](#final-recommender)
- [Conclusion](#conclusion-and-looking-ahead)

# Background
## Bottom Line: Audiences don't care about accurate predictions.

Early recommendation algorithms were evaluated based on accuracy. However more often than not, showing a user recommendations we're sure they're going to rate 5 stars makes for a boring platform.

If a user rates Captain America 5 stars, an accurate recommendation would be every single Avengers movie from there. 

![captainamerica](https://github.com/leo-schell/IntentionalDiversity_RecSystems/assets/122314061/2a4377f9-f4d4-4ce3-9154-525ca2b7611b)

As business analysts, we love to see that dedication to a franchise, however users are more complex than this. In building our models, we need to acknowledge that our users rely on us to navigate our platform and understand all of the options we provide. Furthermore, we owe visibility to the content creators who make our video streaming platforms possible.

# Data Understanding
In 2006, Netflix hosted a [competition](https://en.wikipedia.org/wiki/Netflix_Prize) to see who could beat their most accurate model. I used this data for my project sourced from [Kaggle](https://www.kaggle.com/datasets/netflix-inc/netflix-prize-data).

The original dataset contains 100 million ratings collected from 1999-2005 from 480,189 users of 17,700 movies, shorts, and shows. I was provided with a rating from 1-5, the date of rating, and the video release date which ranged anywhere from 1898 to 2005. With my limited resources I elected to use a sample of 1 Million radomly selected ratings. 

The resulting dataset contains ratings in the same time span from 290,022 users of XXX videos.

## Data Limitations & Initial Diversity Analysis

As with all rating systems, we have very little negative feedback from users. From this data, I cannot ascertain whether users did not rate content because they don't know that it's there, if they watched and simply chose not to rate, or if they intentionally elected not to engage with the content. 

As the data and video content is out of date, this project cannot speak to the current state of diversity on Netflix's platform. I did perform my own **minority representation analysis** where I looked at each video and marked those where Top (2) Billed Cast, Director, or Writer(s) are a minority. White women count as minorities in this instance as they are also underrepresented in the above categories.

The data itself does not contain very diverse observations. **The vast majority of content was released from 2000-2005.**

<p align="center"><img src="images/Product-graph.png" width=60% height=60% alt="bar graph of product"></p>


**User engagement is low as most users only rated once.**

<p align="center"><img src="images/Product-graph.png" width=60% height=60% alt="bar graph of product"></p>


**The content itself only contains 12.4% videos with minority representation.**

<p align="center"><img src="images/Product-graph.png" width=60% height=60% alt="bar graph of product"></p>

## Train-Test Split

In order to better understand how user preferences evolve with time, I employed an 'Out of Time' data split that is typically used with content recommendation systems. See [1M_DataSplit](https://github.com/leo-schell/IntentionalDiversity_RecSystems/blob/main/1M_DataSplit.ipynb) for a step-by-step breakdown of how this was achieved.

# Models Analyzed

I drew a lot of inspiration from Netflix throughout this project. The platform uses many different algorithms because there is a time and place for each type of recommendation. 

With the resources and time that I have available, I was able to create a personalized video ranker and a video-to-video recommendation list. 

### Baseline Model - Biased Baseline

The Biased Baseline model was the baseline model used by [BellKor's](http://snap.stanford.edu/class/cs246-2015/slides/08-recsys2.pdf) solution for the Netflix Prize. As this is an academic exercise and I am seeking to understand recommendation systems on a granular level, I learned a lot from their solution.

This was the baseline model they used. Predictions are calculated using the following formula:

    rᵤᵢ=μ + bᵤ + bᵢ

Essentially, this model operates on the assumption that you can predict a user's rating based on their natural bias. In layman's terms:

User's Rating = (mean ratings for the entire sample) + (the difference in how a user tends to rate videos) + (the difference in the content's own average rating)

This model performed extremely well and was very hard to beat.


## Personal Video Ranker

Historically, collaborative filtering models have been a large part of recommender algorithms. Personal video rankers, collaborative filtering models that seek to predict the content a user will rate highly, usually take prominent places in different capacities on all content delivery platforms.

Following is my best Singular Value Decomposition algorithm, using SVD++ from the same package.

## Video To Video Ranker

In order to promote content diversity, content delivery platforms usually employ models that connect users with content that is similar to what they have been exposed to already.

These models are trained only to examine the similarities between the content available.

I used the kNNBaseline model from the Python Surprise package to start and have left my best performing iteration from there.

# Final Recommender

This begins with the Personalized Video Ranker. If a user's personalized recommendations don't contain a video that meets the minority requirement, the algorithm identify nearest neighbors for the top 10 videos output in the the user's personalized video recommendations.

The algorithm then checks for movies in this pre-curated list of Will Smith, Lucy Liu, and Jennifer Lopez movies:

Will Smith:

Hitch (2005): 17324
Shark Tale (2004): 5345
Bad Boys (1995): 2186
Lucy Liu:

Mulan 2 (2004): 13836
Charlie's Angels (2000): 6552
Jennifer Lopez:

Maid in Manhattan (2002): 11149
Out of Sight (1998): 13486
I have chosen these celebrities as they were very popular in 2005 and tested well in markets throughout middle America and internationally.

If any of the above movies are present among the nearest neighbors, it will boost that movie to the 3rd recommendation in the personal video recommendations.

If none are present, the algorithm will insert Men In Black (2002) as the 3rd recommendation in the personal video recommendations.

Holdout Will Smith Movie:

Men In Black (2002): 12918

# Conclusion and Looking Ahead

This was an academic exercise and I only used algorithms that are used by small businesses and entry level data scientists.

Going forward, I want to get better acquainted with deep learning techniques and the way that major content platforms implement different regularization techniques that impact diversity.

I also look forward to working with bigger more contemporary datasets.
