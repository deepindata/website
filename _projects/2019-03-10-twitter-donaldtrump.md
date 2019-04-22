---
title: 'Is @realdonaldtrump the real Donald Trump'
subtitle: 'NLP to identify difference between tweets from iPhone and Android'
date: 2018-06-30 00:00:00
description: NLP to identify difference between tweets from iPhone and Android
featured_image: '/images/twitter_background2.jpg'
preview_image: '/images/twitter_preview.jpg'
---

<!-- ![](/images/twitter_background2.jpg) -->

I saw a tweet awhile back hypothesizing that when it's actually Donald Trump tweeting from his @realdonaldtrump account, it comes from Android. But when his staff tweets, it usually comes from iPhone. I pulled some of his tweets from the Twitter API to get a look at the metadata and see if there was a noticable difference between the two sources, and, this is what I found: 

**iPhone:**
![](/images/twitter_iphone.jpg)

**Android:**
![](/images/twitter_android.jpg)

No matter what your party affiliation is, I think there's a pretty clear difference between the two groups. So I wanted to use NLP to see if I could separate all his tweets into 2 clusters based on the text in the tweet. And since I have the actual source of the tweet, I can measure how accurate I was in separating the tweets into these 2 distinct groups. I read a data science [blog](http://varianceexplained.org/r/trump-tweets/) that did similar work, but I wanted to try to implement it in Python instead of R.

I could only get the most recent 200 tweets through the Twitter API, but also incorporated a dataset Kaggle had of Donald Trump and Hillary Clinton tweets for a few months over the election season. In the end I ended up with about 2,100 Donald Trump tweets that was split pretty closely between Android - iPhone (52% - 48%). 

I used tf-idf (term frequency - inverse document frequency) to determine the importance of each word in the collection of tweets. So if a word like "the" appears in all the tweets, it's not really that meaningful in helping assign the tweet to one of the clusters. As you can imagine, this results in A LOT of words, the vector of the resulting tf-idf will be as long as the # of unique words across all of Donald Trump's tweets. 

To help reduce the size of this vector, I used a technique called Latent Semantic Analysis (LSA). This helps reduce the length of the vector by basically combining some of the words that often appear together into a single feature. A super simplistic example based on the tweets above, if "Obamacare" and "disaster" often appear together in the same tweet, LSA would essentially combine them into one feature, and the tf-idf vector would shorten. Once LSA is performed, the tf-idf vector is a lot more manageable, and I have a matrix that assigns each tweet to the iPhone cluster or the Android cluster. 

Once the tweets were clustered, I calculated the accuracy score and got 79%. Seems like it's pretty predictable which were coming from Android and which were coming from iPhone. 

I wanted to look more into the material of each tweet to see if there were certain keywords that could help identify who wrote the tweet. I looked up the most important words from each category (based on LSA), and found the following: 

![](/images/twitter_words.jpg)

Looking first at the iPhone cluster, it looks like tweets with a link (red), with hashtags (green), and with locations (blue) typically came from iPhone. I assumed multiple words strung together without spaces were hashtags, and locations are probably so predominant to advertise for rallies leading up to the election. 

Looking at the Android cluster, it looks like people were mentioned more (red) and there were a lot more emotional words used (blue). My guess is that people being mentioned means they were likely on the receiving end of a Trump twitter attack, and the emotional words are pretty characteristically Trump even from the snapshot of Android tweets I listed above. 

