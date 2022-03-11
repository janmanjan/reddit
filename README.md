# Project 3: Web APIs & NLP

### Problem Statement
Given a post from two similar subreddits, can I build a model, using NLP, to correctly classify which the subreddit the post originated from with at least 80% accuracy?

#### Background
Reddit is a "network of communities where people can dive into their interests, hobbies and passions". It's a collection of forums where people can share content(videos, images, ideas, etc.). Posting on the subreddits, a community/interest specific forum, consists of a title and its selftext wherein the community can respond via comments. 

The two particular subreddits of interest for classification are r/NoStupidQuestions and r/TooAfraidToAsk. The first subreddit is "for people to feel free to ask the questions they might be embarassed or ashamed to ask elsewhere". An example post is "Do you think it's toxic for parents to take away a child's door?". The second subreddit is for "everything and anything you were too afraid to ask". An example post is "Why are white Americans almost never called 'European Americans' in the same way black Americans are almost always called 'African Americans'?. Although both subreddits are similar in terms of theme, both handle a diverse set of topics. 

Natural Language processing (NLP) refers to a branch of artificial intelligence or AI concerned with transforming or vectorizing text. NLP converts given text such that a computer can read and analyze what's given in order for the data to be used on various statistical models. Common examples include text analysis, smart assistants, email filters, and language translation. This project will pull the most recent posts starting from 1/15/22 12:00pm, from the subreddits of interest, using the [*PushShift API*](https://github.com/pushshift/api). Analysis and modeling will be done on posts containing a title and its selftext such that the models can use as much text as possible in order to classify the origin of the post. 

Finding a classification model for these subreddits via their post and selftext can potentially determine where certain topics can or should be posted. However, considering the descriptions for each subreddit, it may not particulary matter. Nevertheless, this would be a good avenue for testing some limitations of NLP or at least the amount of conditions NLP might need to best be utilized. 

### Data Analyzed
* ['subreddits_cleaned.csv'](./data/subreddits_cleaned.csv):
From ['r/NoStupidQuestions'](https://www.reddit.com/r/NoStupidQuestions/) ([source]) (https://api.pushshift.io/reddit/search/submission?subreddit=NoStupidQuestions): About 60,000 posts were used
From ['r/TooAfraidToAsk'](https://www.reddit.com/r/TooAfraidToAsk/) ([source]) (https://api.pushshift.io/reddit/search/submission?subreddit=TooAfraidToAsk): About 60,000 posts were used

### Data Dictionary
|Feature|Type|Dataset|Description|
|---|---|---|---|
|subreddit_nsq|str|subreddits_cleaned|the subreddit that the post was originally posted, 1 for r/NoStuipdQuestions and 0 for r/TooAfraidToAsk|
|title|str|subreddits_cleaned|title or question posted on the subreddit|
|selftext|str|subreddits_cleaned|the background information or body text provided for the post|
|created_utc|int|subreddits_cleaned|the json timestamp of when the post was posted|
|text|str|subreddits_cleaned|The combination of title and selftext|


#### Analysis, Conclusions, and Recommendations
The two subreddits had a significant amount of overlap in terms of text; a model with at least .8 accuracy could not be achieved with NLP. The best model generated had an accuracy of .69 despite noticeable word count differences for shared words per each subreddit. No model out of the 9 used nor the random search or the bayesian search was able to rise above .69. Stemming, expanding contractions, and optimizing the threshold for misclassification of positive values improved scores but not enough to push accuracy above .69.

After looking that the top 15 words for each subbreddit and considering all the words the subbreddits shared, it seems clear that there was a ceiling for the model accuracy scores. Despite the variance in topics, because users were asking questions and therefore using similar phrasing, the novel words that were used for specific topics could not affect the model significantly due to sheer volume. Although models performed similarly in terms of accuracy scores, the behavior of the last two compared (Multinomial Naive Bayes & Logistic Regression) produced different classification metrics. This result, although not extensively explored, is likely due to the different mechanics behind the models. Logistic Regression was picked as the last model due to speed, efficiency in resources, and the ability for the model to be more readily explored in terms of analysis. 

For data sets that are similar with regards to text, I suggest using different models to assess classification. For instance, a simple logisitic regression model using only created_utc as a feature outperformed all my models with an accuracy score of .85. Another alternative would be to include topic variables such as link_flair_text in the api in order introduce overall themes of posts. 
