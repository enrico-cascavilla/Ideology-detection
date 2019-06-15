
# CAPSTONE PROJECT IDEOLOGY DETECTION

## Executive summary

The project consist to detect a political Ideology from a quote or a tweet.
In the world we are leaving social media have a very important role in our life. The speach in streets are disappearing and many of politician use social media to delivery message to the country. Seems also that more and more people are less interested in political debate in the parliament and follow politics and politician through twitter, facebook or any other social media who use a short synthetic sentence.
The idea behind is about the philosophical idea of the truth. A political party is historically inspired from an ideology, but often politicians float and play with that to get more consent.
I want to try to predict the ideology behind a political quote / a political tweet / political post.

The project is for english language, so most of the authors have english as their mother tongue. Very few of them are not native english speaker(For example: Yanis Varoufakis, which is Greek-Australian), but since their international personality they are writing mainly in english and they live/work in an english environment.

To accomplish my goal I will use other quotes from people who can be identified in an ideology or represent it.

## The Dataset

According to Wikipedia roughly twenty main political ideology can be identified (https://en.wikipedia.org/wiki/List_of_political_ideologies). Inside those many variations of the same are present, most of the variation refers to a geocraphical position, or single person. 
Some of these ideology relate only to specific topic.

The usual political spectrum is oftnen represented in a 2D graph where the x-axis is the "common" Left-Right spectrum, and the y-axis is the freedom axis (a good example is in the website called political compass https://www.politicalcompass.org/analysis2?ec=-4.88&soc=-6.77). 

For this project I considered only the x-axis, the Left-Right spectrum. Evey position will be projected on the x-axis.

The spectrum I am going to use it is divided in five main section : 
- Left (Comunism, Socialism, left Anarchism, new Ambientalism, left Populism)
- Progressive (Social democratic, democratic)
- Moderate (Indipendent, Popularism, Christian democracy)
- Conservative(Conservative, Conservative Populism) 
- Right(Fascism, Alt-Right, right Populism)

For each category I selected a list of not-politician:
- Left: Noam Chomsky, Yanis Varoufakis, Jeremy Rifkin, Naomi Klein, John Bellamy Foster, Raj Patel, Michael Parenti, Michael Hardt and Antonio Negri, George Monbiot, John Gray, Michel Chossudovsky, Perry Anderson, Alexander Cockburn, Terry Eagleton, David Harvey, Fredric Jameson, Raymond Williams, E. P. Thompson, Eric Hobsbawm, Tariq Ali, Angela Davis, Mumia Abu-Jamal, Howard Zinn, Judith Butler, John Holloway, Michael Moore, Michael Kazin, Richard D. Wolff, Steve Ellner, Branko Milanovic, Anthony Barnett, Micheal Otsuka, Jodi Dean, Abby Martin.


- Progressive: Juan Williams, Bill Moyers, Paul Krugman, Joseph E. Stiglitz, Jesse Jackson, George Soros, Donna Brazile, James Carville, Tony Judt, Jane Jacobs, Lawrence Lessig, Glenn Greenwald, Al Gore, Alan Bennett, Anthony Giddens, Benjamin Barber, Robert Reich, Simon McKay, Ezra Klein, Nicholas Kristof, Sam Harris, Polly Toynbee.


- Moderate: Robert Kagan, Leon Kass, Jonathan Haidt, Ian Bremmer, Thomas Friedman, Peter Drucker, David Brooks, John Avlon, Mark Satin, Arianna Huffington, Ben Bernanke, Joseph Nye, Stephen Walt, Fareed Zakaria, Tibor R Machan, Chris Matthews, David Rockefeller, Peter Thiel, Josh Marshall, Jeffrey D. Sachs, John Rawls, Edward Snowden, Erik Wemple, Maureen Dowd, Lawrence H Summers.


- Conservative: William F. Buckley Jr.,George Will, Bill O'Reilly, Barry Goldwater,Andrew Sullivan, Robert P George, Roger Stone,Paul Manafort, Milton Friedman,Thomas Sowell,Charles Murray, Kevin D Williamson,Walter E Williams, Robert Lucas Jr,Andrew Breitbart, Bill Kristol,David Friedman, William Happer, Ben Stein, Glenn Beck,Mona Charen,David Frum, David Horowitz, Jeane Kirkpatrick,Charles Krauthammer, Irving Kristol, W. Cleon Skousen,Rush Limbaugh,Tucker Carlson,Andrew Napolitano,Roger Kimball,Michelle Mallon,David Clarke.


- Right: David Goodhart, Ben Shapiro, Jared Taylor, David Duke, Matthew Goodwin, Eric Kaufmann, Richard Bertrand, Augustus Sol Invictus, Peter Hitchens, David Irving, Jason Kessler, Sebastian Gorka, Tomislav Sunić, Paul Weyrich, Pat Buchanan, Steve Bannon, Ann Coulter, Milo Yiannopoulos, Vox Day, Steve Sailer, Stefan Molyneux, Alex Jones, John Derbyshire, Mike Cernovich, Peter Brimelow, Katie Hopkins, Laura Loomer, Paul Joseph, Arthur Kemp, Tommy Robinson, Raheem Kassam, Jerome Corsi, Daniel Drezner, Lou Dobbs, Pamela Geller, Robert Spencer, Karl Hess, Hans-Hermann Hoppe, William Luther Pierce, Joel Pollak, Matt Drudge, Nicholas Wade, John R. Bolton, Dinesh D'Souza, Charlie Kirk.


The label of a particular category is given from:
- Wikipedia: In the personal summary of the person, or in the paragraph "political view"
- Allside.com : A webside that label the political bias of newspaper and journalist



----------------------------------------------------------------------------------------------

NB: Many of the people from the lists are not in the Database. The names come from my research following the criteria. The lists have been made before I took the data and suppost to provide also for Twitter Database, which I did in part. Unfortunately the Twitter corpus presented a mistake in the code that provided me only half tweet. Due the luck of time I couldn't scrape again. 

### Quotes

I created a dataset using just quotes scraped from three particular website: Wikiquote, Brainquote and Goodreads.
I used a python library, wikiquotes, to get the quote from Wikiquote. Having a different Jupyter Notebook for each Category. 
Later I create a new Jupyter notebook where I scraped the quotes from the other 2 website(Goodreads and Brainquote) and I joined the 3 dataset, I wrote the same name (string) for same author if different.

At the end I randomly selected few authors to have a new test dataset

## Problem and consideration about the Data

##### Quotes

- Not all the quotes are inherent, but most of it. So I am expecting a lot of "noise".
- The dataset is very limited. I am going to work with just bit more than 1500 quotes for each category. 
- I am considering the last 60 years for the quotes. Many quotes are of course related to the politics of the period, and for each category we can see a sort of bias for the time period. 
- The category are balanced in the number of quotes but not in the length, and each category has an imbalance in the quotes by Authors, with the risk that the model will overfit with few of them.
- Since I have different number of quotes for each category. To create balanced class I will select randomly the same amount for data for each one. The results could slightly change everytime we select a new bunch.

## Cleaning

The main problem I met (apart of finding a considerable amount of quotes) has been the cleaning of the quotes. Since the quote online are written from users and not filtered, there are many duplicates. Especially small quotes often were present in larger quote, and sometimes with few little change in the writing (articles, preposition and punctuation).
To find them I looked if a single string was present in any quotes.
Then I did the same operation but using a clean pre-processed text (with textacy).
And for last same operation using the Countvectorizer with a fairly big gram (5-6 words).
I probably I could have used only the last one, but at beginning I didn't realize all the possible little differences between the quotes (that I mentioned before).
Since all these steps were not enough to remove all the duplicates I had to do a last drop manually, using the gram system and the method str.contains(word in gram).
I also operated a fourth iteration to look for quotes about the authors and not from the authors. Some of these quotes could have been still interesting, but since I couldn't check manually all of them I chose to remove them. I couldn't read most of the quotes, but I read some. Whenever I was founding a noisy, useless quote I removed manually. For the same reason I chose to remove few authors (For example Katie Hopkins) which after removing few noisy quote I investigated.

Since the median of the quotes is larger than 30 words in every category I chose to eliminate every quote with less than 8 words.
As last step of the cleaning I applied lemmatization.

## Baseline and Vocabulary

To create balanced classes I took the same number of random data for each class, the number of the quotes has been chosen from the number of the weakest category (Progressive).
The baseline is 0.2

Investigating the vocabulary for each class we realized immediately that the goal will be very hard as most of the frequent words are the same, in each category.  The first most used word by category is the same for each class: "people" for the quote, the second is "world" in three category and in the top 5 in the left two.  We can see that the two extreme category have more unique words and concepts, and we notice also that many category contains words opposite to their ideology. This happen because they talk about other categories.

The extreme ideologies have more words, and median and mean of lenght of quotes bigger. Moderate take the last position in all these ranking.

## Modelling

I started creating a pipeline using CountVectorizer. After the inspection of the vocabulary I increased the stopwords with other few not very usefull words that came out in the top 30 (auxiliary verbs especially).
Logistic Regression is the first model to try in the pipeline: 
A score around45%-46% on the test set show us that a pattern could be found in the words. From the confusion matrix we can clearly see the diagonal of the true value, and often the neighbor category has the highest values in the false negative.
Since the final dataset is created random, I chose to not save it and run a different dataset everytime. 
After several observations I can say that the difference is very little every time. 
Conservative is the class with the lowest value of precision and recall. In some particolary situation of the random dataset, the left became the first false negative for the conservative. From the most important coefficient we notice that in the left we have few words that describe the conservative (capital and neoliberal) and conservative have comunist. At the same time, there are some similarity in the negative coefficients. Overall the coefficients start to track a line and give us some boundaries for the categories.

After trying other classifiers, the best score happen to be with MultinomialNB. But if the score is slightly better, confusion matrix, precision and recall became more imbalanced. The previous problem between left and conservative became bigger. One of the reason of this mismatch is that the machine predict left more than other categories. In fact left has a pretty low precision (only conservative have a worst precision) and the higher recall.
Words like left, marx, happy, history...push the choice to the left; America, republican to conervative; Israel to the right; books to the progressive and topic about morality in the centre. So if Noam Chomsky use the word Israel will be label as Right category. 

The result on the little external dataset are lower. This show that our model overfit a little  on the authors, but confirm mostly what we saw in the other database. Generally, the model doesn't behive too bad at the extreme, but is more confuse in the centre. 

We can explain this behavior also from real life. The extreme are the categories that want to change the status quo, while the categories in the middle, in different ways, want to keep it.
And we could notice this almost from the beginning and the most frequent words.

Using the TfiVectorizer we see a little improvement in the score, both with Logistic Regression and MultinomialNB. The coefficients also didn't change that much. Overall the behavior of the machine is very similar. On the test set, we have a good improvement to indentify the class moderate and still decent on the extreme. 
Looking again at the coefficients we can notice that Progressive are different from the left for beacue are more related to goverative and economic words.
The worst category to identify is the again the conservative which finish in the right (an error that can be acceptable in same cases) and left. most likely for the same reason we met with the Countvectorizer.
We also notice that some "family" related words appear in the right side of the spectrum. So every quote that contain the words child, father, are probaly categorized as conservative or right. The right keeps words typical of nationalism and of the far right : muslim, islamic, israel, jews.
Exactly like the machine didn't change much in the big dataset, also in the external dataset almost everything we notice in the previous analysis is confirmed.

Using a threshold confusion matrix I built we confirm even more that the strongest probability are in the extreme categories. The reason is again in the different words used from these categories. This shows also, that the confusion that the machine has for the 3 categories in the centre,  comes from low probabilities.

The Precision Recall Curve and the ROC, shows Left and Right above any other, and centre in middle. Not great for Progressive and Conservative. These last two categories are sometimes even lower than the baseline in the external dataset, probably due to the overfitting of the authors.


#### Mentioning other methods that have worsened the result

As last things I tried Sentiment Analysis and Word2Vec. Sentiment Analysis with Vader brough a massive bias on the Progressive class. In the external dataset almost every prediction is Progressive. A tools that in this case didn't help and I am not sure can improve in a case of ideological detection.
Word2Vec had a lower score, and also every class perform slightly worst than the other models. In the external dataset predict with great precision and recall the Left, but the rest is very confusing. I don't think that this tool is useful with this kind od dataset. 

# Future work and Conclusion

- Our data is very noisy. A manual labelling would definetely tune the machine. To keep the objectivity, maybe some expert or student of science politics / politics, would create an interesting dataset. 
- To accomplish this would be better have a better research not only with the quotes available on the web, but from books and accademic paper.
- The labelling by author can be good but it has a big limitation: It is always possible that the mind of the person, usually more coherent than most of politician's mind, can float. I feel that everytime we try to have a rigid classification about something not so rigid, like human mind, is forced and no natural. A biased author not always will have a biased content.
- The machine evaluate at the same way a close (acceptable) error with a far category. A method for ordinal classification could fit better our problem.
- To define such a complex topic require more more data. The machine works mainly combining words. More combination we use to feed the machine more likely the opinion mining can be better.
- Since I use the last 60 years, the machine not overfit to the last period time, actually probably underfit. At the same time, new and specific topic, like brexit are meaningless.
- Could be interesting feeding the machine with completely different data: like newspaper articles, or tweets. The main problem using tweets is that the noise would be even bigger. Manual labelling would be the best again. The problem using newspaper articles is that would be difficult to classify a social medie post or tweet. 
- Mentioning my last 2 steps, Sentiment Analysis could come useful in a Dataset with Tweets, maybe splitting before, while Word2Vec I think can come handy in a dataset with complex but organized corpus like newspaper articles or blog articles.

## Conclusion

I can conclude that the machine shows a quite clear behivior. It has the main problem of not understanding the sentiment, and unfortunately Vader didn't help. Using a certain word with a strong weight will classify your tweet in that particular category, without understanding if the opinion is in favour or against. If you use the word "muslim" or "israel" you will be most likely classify as far right.
Metaphors are also a weak point. But they are a weak point also for many people.
Ideology is something difficult to understand also in real life, it has been 


