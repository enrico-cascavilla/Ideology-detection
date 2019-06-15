
<a id='conclusions-futureimplementations'></a>

# Conclusions

- The data is very noisy. Manual labeling would definitely tune the machine. To keep the objectivity, maybe some expert or student of science politics/politics would create an interesting dataset. 
- To accomplish this would be better to have better research not only with the quotes available on the web but from books and academic paper, newspaper and blog.
- The labeling by authors can be good but it has a big limitation: It is always possible that the mind of the person, usually more coherent than most of the politician's mind, can float. I feel that every time we try to have a rigid classification about something not so rigid, like the human mind, is forced and no natural. A biased author not always will have biased content.
- The machine evaluates at the same way a close (acceptable) error with a far category. A method for ordinal classification could fit better our problem.
- To define such a complex topic it requires more data. The machine works mainly combining words. More combination we use to feed the machine, more likely the opinion mining can be better.
- Since I use the last 60 years, the machine not overfit to the last period time, actually probably underfit. At the same time, new and specific topics (like Brexit) are meaningless.
- There is a big homologation in the political lexicon. Especially in the 3 central ideologies.
- There are words like "right" which are very difficult to interpret, due to many meaning that the word could have. Metaphor and humor are a big problem too.
- There is a patter that needs to be investigated to get better result.

# Future implementations

- Better Dataset, different Dataset: Quotes without noise, investigate the same method with a Twitter database, and newspaper/blog database.
- Use topics as part of the features
- Locate the Database in the space and in time: Use English was important to collect more data but to improve, create a model for each country (space) and for each topic could bring a massive evolution in the project. Exactly we should locate the database in a shorter period of time.
- Create a specific metric evaluation that could be compatible with this ordinal classification problem. Bespoke confusion matrix and classification report.
