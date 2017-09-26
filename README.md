# Twitter-Fast-Food-Predictions

To look at all the code and steps view the notebook using nbviewer: 
https://nbviewer.jupyter.org/github/cgunders/Twitter-Fast-Food-Predictions/blob/master/Twitter-Fast-Food-Predictions.ipynb

## 1. Introduction

Toward my goal of becoming a data scientist I started a personal project. I initially began with learning syntax and simple algorithms through online coding quizzes. Eventually I ended up with the Kaggle Titanic dataset and progressed through the cleaning, feature engineering, data visualization, and machine learning. It was a good introduction to the process but I wanted to experiment with my own data collection and analysis. I decided to collect Twitter data as it is publicly available and contains a wealth of information that can be scraped from their website with relative ease. Some of this data is messy and must be cleaned up. Examples of this are tweets that contain non-ASCII characters, emojis, and web links. I had read several articles about how data scientists must collect or use data which is messy and must be cleaned into a usable format for analysis. I wanted to mimic this by collecting my own messy data set and cleaning it.
  
I wanted to address a question that I found interesting but could also be applicable from a business perspective. I wanted to categorize tweeters of fast food restaurants and develop a model that could determine which fast food chain or fast food category (burgers, fried chicken, pizza) an uncategorized tweeter would prefer. This is a simple model and has several assumptions to make it practical for me to complete. This idea, I assume, is used in targeted advertising across many social media platforms and is not new in any way. An example given by Twitter themselves is at: https://dev.twitter.com/ads/campaigns/targeting. 
  
## 2. Method
 
There are many articles available on the internet that discuss obtaining and dissecting Twitter data and for the most part they use the Twitter API which packages tweet and user data up into a JSON. I chose a different route, using Beautiful Soup to extract the data from the HTML on Twitter’s website. This is a more limited approach for obtaining Twitter data, but it can be broadly applied to any website on the internet, even if it lacks an API. There are articles using this method as well, which I used as a guide. 
  
Choosing to gather Twitter data without using their API presents a few problems. Notably, Twitter only loads 20 tweets on the initial query and a user must scroll down to load more. To circumvent this problem, it is common to use Selenium to control a web browser and mimic a user scrolling through tweets. When the desired number of tweets have been loaded the HTML document is parsed with Beautiful Soup and data is extracted. I also used Joblib to run multiple instances of my scraper in parallel. With four cores available on my laptop this sped up data acquisition by a factor of about 4.
    
I chose this method to learn about more than just Twitter’s API. By constructing my own scraper I used different packages with a diverse range of applications that will aid me in future projects.

The Twitter scraper that I wrote can be found at: https://github.com/cgunders/Beautiful-Soup-Selenium-Twitter-Scraper
    
## 3. Results

After scraping, features are engineered from the raw data. Features I use for principal component analysis (PCA) and machine learning are: number of retweets, number of replies, number of likes, number of hashtags, number of atreplies, tweet length, day of week, time of day, if the tweet contains a picture, if the tweet contains a link, if the tweeter is verified, and the subjectivity and polarity of the tweet as determined by sentiment analysis with TextBlob. 

Sentiment analysis of TextBlob showed some big differences between the fast food queries and the javascript query. People talking about javascript tended to use more neutral and less negative words compared to the fast food queries.

| Query         |Fraction Positive|Fraction Neutral |Fraction Negative|
|---------------|:---------------:|:---------------:|:---------------:|
|Burger King    |0.308            |0.526            |0.167            |
|McDonald's     |0.405            |0.401            |0.194            |
|Chick-fil-A    |0.380            |0.446            |0.174            |
|KFC            |0.309            |0.475            |0.216            |
|Javascript     |0.355            |0.566            |0.079            |

![alt text](https://github.com/cgunders/Twitter-Fast-Food-Predictions/blob/master/chickfilapcorr.png "Chick-fil-A Pearson Correlation")

![alt text](https://github.com/cgunders/Twitter-Fast-Food-Predictions/blob/master/javascriptpcorr.png "Javascript Pearson Correlation")

![alt text](https://github.com/cgunders/Twitter-Fast-Food-Predictions/blob/master/good_dfsPCAPlot.png "Scatter Plot of Principal Components of Queries")
