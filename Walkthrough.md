
<a id='Walkthrough'></a>

# Walkthrough

### Problem and consideration about the Data

- Not all the quotes are inherent, but most of it. So I am expecting a lot of "noise".
- The dataset is very limited. I am going to work with just a bit more than 1500 quotes for each category.
- I am considering the last 60 years for the quotes. Many quotes are of course related to the politics of the period, and for each category, we can see a sort of bias for the time period.
- The categories are balanced in the number of quotes but not in the length, and each category has an imbalance in the quotes by Authors, with the risk that the model will overfit with few of them.
- Since I have a different number of quotes for each category., to create balanced classes I will select randomly the same amount for data for each one. The results could slightly change every time we select a new bunch.

### Cleaning and Preprocessing

The main problem is the cleaning of the quotes. Since the online quotes are written by users and not filtered, there are many duplicates. Especially small quotes were often found in a large share, and sometimes with few changes in the writing (articles, preposition, and punctuation).
To find them I looked if a single string was present in any quotes.
Then I did the same operation but using a clean pre-processed text (with textacy).
And for last same operation using the Countectorizer with a fairly big gram (5-6 words).
I probably could have used only the last one, but at the beginning, I didn't realise any little differences between the quotes (that I mentioned before).
Since these steps were not enough to remove the duplicates I had to do the last drop manually, using the gram system and the method str.contains (word in gram).
I also got the fourth iteration to look for quotes about the authors and not from the authors. Some of these quotes could have been still interesting, but since I could not check them manually. I could read most of the quotes, but I read some. Whenever I was founding a too noisy, useless quote I removed manually. For the same reason, I have investigated. (For example Katie Hopkins)

Since the median of the quotes is larger than 30 words in every category I chose to eliminate every quote with less than 8 words.
As last step of the cleaning, I applied lemmatization.

### Baseline and Vocabulary


To create balanced classes I took the same number of random data for each class, the number of quotes has been chosen from the number of the weakest category (Progressive).
The baseline is 0.2

Investigating the vocabulary for each class we realised immediately that the goal will be very hard as most of the frequent words are the same, in each category.  The first most used word by category is the same for each class: "people" for the quote, the second is "world" in three categories and in the top 5 in the left two.  We can see that the two extreme categories have more unique words and concepts, and we notice also that many categories contain words opposite to their ideology. This happens because they talk about other categories.

The extreme ideologies have more words, and median and mean of length of quotes bigger. Moderate take the last position in all these ranking.

### Modelling

I started creating a pipeline using CountVectorizer. After inspecting the vocabulary, I increased the stopwords with other few not very useful words that came out in the top 30 (auxiliary verbs especially).
Logistic Regression is the first model to try in the pipeline:
A score around45%-46% on the test set show us that a pattern could be found in the words. From the confusion matrix, we can clearly see the diagonal of the true value, and often the neighbor category has the highest values in the false negative.
Since the final dataset is created random, I chose to not save it and run a different dataset every time.
After several observations, I can say that the difference is very little every time.
Conservative is the class with the lowest value of precision and recall. In some particular situation of the random dataset, the left became the first false negative for the conservative. From the most important coefficient, we notice that in the left we have few words that describe the conservative (capital and neoliberal) and conservative have communist. At the same time, there is some similarity in the negative coefficients. Overall the coefficients start to track a line and give us some boundaries for the categories.

After trying other classifiers, the best score happens to be with MultinomialNB. But if the score is slightly better, confusion matrix, precision, and recall became more imbalanced. The previous problem between left and conservative became bigger. One of the reasons for this mismatch is that the machine predicts left more than other categories. In fact, left has a pretty low precision (only conservative have the worst precision) and the higher recall.
Words like left, Marx, happy, history...push the choice to the left; America, republican to conservative; Israel to the right; books to the progressive and topic about morality in the center. So if Noam Chomsky uses the word Israel will be label as Right category.

The result of the little external dataset is lower. This show that our model overfit a little on the authors, but confirm mostly what we saw in the other database. Generally, the model doesn't behave too bad at the extreme but is more confused in the center.

We can explain this behavior also from real life. The extreme is the categories that want to change the status quo, while the categories in the middle, in different ways, want to keep it.
And we could notice this almost from the beginning and the most frequent words.

Using the TfiVectorizer we see a little improvement in the score, both with Logistic Regression and MultinomialNB. The coefficients also didn't change that much. Overall the behavior of the machine is very similar. On the test set, we have a good improvement to identify the class moderate and still decent on the extreme.
Looking again at the coefficients we can notice that Progressive are different from the left because are more related to government and economic words.
The worst category to identify is the again the conservative which finish in the right (an error that can be acceptable in same cases) and left. most likely for the same reason we met with the Countvectorizer.
We also notice that some "family" related words appear in the right side of the spectrum. So every quote that contain the words child, father, are probably categorised as conservative or right. The right keeps words typical of nationalism and of the far right : muslim, islamic, Israel, jews.
Exactly like the machine didn't change much in the big dataset, also in the external dataset almost everything we notice in the previous analysis is confirmed.

Using a threshold confusion matrix I built we confirm even more that the strongest probability are in the extreme categories. The reason is again in the different words used from these categories. This shows also, that the confusion that the machine has for the 3 categories in the centre,  comes from low probabilities.

The Precision Recall Curve and the ROC, shows Left and Right above any other, and centre in middle. Not great for Progressive and Conservative. These last two categories are sometimes even lower than the baseline in the external dataset, probably due to the overfitting of the authors.

### Voting

After a careful manual evaluation of the misalignments, I noticed that each model behaves in a partially different way. All retain a discreet consistency. Because of this behavior, we choose to use a forecast type of media using a voting classifier. although the score only increases in the external dataset, there are slight corrections and some decisions become more tolerable.

### Mentioning other methods that have worsened the result

As of last things, I tried Sentiment Analysis and Word2Vec. Sentiment Analysis with Vader brought a massive bias on the Progressive class. In the external dataset, almost every prediction is Progressive. A tool that in this case didn't help and I am not sure can improve in a case of ideological detection.
Word2Vec had a lower score than the main models analysed, and also every class performs slightly worse than the other models. In the external dataset predict with great precision and recall the Left and Right, but the rest is very confusing. It needs further analysis, but it is still a new method for me, so it will be part of future implementation.
