
# Project 3 (Webscraping & NLP)

### Problem Statement
With the advent of social media, we are now more interconnected than ever before. This connectivity comes at a price; misinformation is rampant on the internet, and becomes a tool for fearmongering, economical, or even political manipulation. 

As a team of data scientist hired by a US government agency, our group will be looking at the headlines of articles from a popular satirical digital media company, The Onion, and how it compares headlines of real news articles. Our goal to train a model to be able to tell apart real news headlines (gathered from the r/news subreddit) from fake news headlines (gathered from the r/TheOnion subreddit). The insights from this model could potentially allow us to identify patterns in headlines that enable us to quickly flag fake news articles.

### Contents
- [Problem Statement](#Problem-Statement)
- [Data Description](#Data-Description)
- [Modelling](#Modelling)
- [Conclusions](#Conclusions)
- [Recommendations](#Recommendations)

### Data Description
10,000 headlines were scraped from each subreddit for a total of 20,000 headlines. After removing spam submissions from the dataset, I was left with 9336 unique r/news headlines and 8872 unique r/TheOnion headlines. The raw scraped data was saved to two .csv files included in the repository. These are the news_df.csv and onion_df.csv datasets. After the raw data was cleaned, the cleaned data was saved to a third .csv file, which is the cleaned_df.csv dataset.

The data included 3 main features, the headline, its length, and when it was posted. During EDA, a troubling flaw was discovered in the dataset. Due to high volume of submissions in r/news, the 10,000 r/news headlines were all posted in the last 3 months. However, due to lower volume of posts in r/TheOnion, headlines in the r/TheOnion dataset went as far back as September 2017. When common words in the headlines of each subreddit were visualized, it was clear that r/news headlines had a heavy focus on more recent events like the pandemic, which were underrepresented in The Onion headlines.

### Modelling
A first set of models were fitted to the entire dataset. I first fit Bernoulli, Multinomial, and Gassian Naive Bayes models to the dataset. The Multinomial Naive Bayes model had the highest 5-fold cross validation score of 87.6%. After tuning the hyperparameters of the multinomial Naive Bayes model, the model achieved an 5-fold cross validation score of 87.8%.

Subsequently, I decided to fit a logistic regression. During EDA, I observed that many of the common words, especially in the r/news dataset, were heavily correlated with one another, which violates the assumptions of the Naive Bayes model. As logistic regressions do not assume feature independence, I chose to fit a logistic regression model with no penalty and another with ridge regularization. The ridge regression model outperformed all the other models with a final F1 score of 88.4%.

After analyzing the headlines that had been misclassfied, I found that due to the uneven timespans of the dataset, the model was overwhelmingly misclassifying The Onion headlines that mentioned recent events (like the pandemic) as real news articles. As such, I decided to run a final set of models on a subset of data, which only included headlines in the last 3 months. I thus extracted all The Onion headlines that had been posted in the last 3 months, which amounted to 235 headlines, and randomly drew 250 headlines from the r/news dataset to create an evenly split sample. As there were only 485 rows to 2493 columns, I fitted an SVM model to the data, as SVM models perform better on smaller datasets with many features. The model returned a final F1 score of 70.2%.

### Conclusions
Our goal was to train a model to tell apart real news headlines (gathered from the r/news subreddit) from fake news headlines (gathered from the r/TheOnion subreddit). We hoped that this model could enable us to quickly flag fake articles in real-time. Although the first set of models had high F1 scores at around 87% to 88%, analysis of misclassified headlines suggested that the model would not be generalizable to newer headlines. Thus, they would not be able to achieve this goal. 

Model 2 aimed to overcome this hurdle by training an SVM model on a subset of headlines only from the last 3 months, split evenly between r/news and r/TheOnion headlines. Model 2 performed more poorly than Model 1 with an F1 score of 70.2% on the test data. However, an inspection of the wrongly classified headlines showed that significantly less headlines on recent news (like pandemic related headlines) had been wrongly classified. Model 2 thus seemed more effective for our intended purposes.

However, it is also important to note that Model 2 was still trained specifically on The Onion headlines, which tend to be written with a distinct sense of dry humor. The model may not be as effective at picking out sensationalist fake news that are more malicious in nature.

### Recommendations
We thus recommend that the following steps be taken:
- Compile annotated datasets that include fake news articles from a wider variety of sources.
- Evaluate Models 1 and 2 against these annotated datasets.
- Fit new models to the new data if Models 1 and 2 perform poorly.
- In the event that annotated datasets cannot be sourced, put together a team to review if unsupervised machine learning methods can address our problem statement.

In the meantime, it is not uncommon for The Onion articles to go viral after being mistaken as real news, especially after being shared via social media or messaging apps without a source. We should thus make efforts to remind citizens that news snippets without sources should not be trusted. A Google search may provide further insights to the veracity of the article. In some cases, it may simply be another The Onion article gone mistakenly viral.
