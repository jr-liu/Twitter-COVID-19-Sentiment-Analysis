# Twitter-COVID-19-Sentiment-Analysis
## Introduction
Recently, coronavirus is the most eye-catching topic in the world. The confirmed cases in the United States are having an exponential growth, which worries U.S. residents. People are talking about all kinds of issues related to coronavirus on social media, especially on twitter. Such issues range from testing cases to even politics, which are worthy of study. The reason why choosing twitter to do the research is its rapid efficiency to share and receive information and its large numbers of users across the country. There are great amounts of tweets when searching the keywords of coronavirus, like relevant tags of “COVID-19” and “coronavirus”, and they are updating every second. Thus, it offers a solid dataset to analyze Americans' attitudes on coronavirus. By taking a close look at the confirmed cases, we observed a huge variation among states. Since the severity differs among states, we are curious about the influence of increased confirmed cases of COVID-19 on people’s opinions and attitudes on twitter.

In that case, the hypothesis of our project is that there is a potential relationship between topics about COVID-19 on twitter and daily new confirmed cases of different states. In order to explore this hypothesis, this project will first respectively present the word clouds of trending words on COVID-19 of 24 different states via a Shiny App, and then analyze the relations between tweets topics and daily new confirmed cases through scatter plot and multiple linear regression models.

## Methodologies
We used the “rtweet” package to collect 53,213 tweets at multiple times on May 5th, 2020. Since we specifically wanted to study the tweets that directly mentioned coronavirus or covid, the keywords of our data collection were set to be “coronavirus” or “covid”. Also, we targeted to study American Twitter users who set their location information publicly, so we set the location to be the United States, and “rtweet” would automatically return the tweets with the location information in the US. Besides, the dataset we collected includes the user ID, the location information of the users, the content of their posts and etc.

After collecting the raw data, we specified the range of our study to the 24 states in the US. We set the total confirmed cases in Wisconsin as a baseline number (around 9k), so any other state that has a higher number will be considered in the project. Specifically, we use the “grep()” function to categorize the whole dataset into different states. The 24 states include New York (4666), New Jersey (628), Massachusetts (1104), Illinois (1644), California (6960), Pennsylvania (1293), Michigan (806), Florida (2001), Texas (3190), Connecticut (406), Georgia (1137), Louisiana (388), Maryland (622), Indiana (376), Ohio (1017), Virginia (735), Colorado (534), Washington (1409), Tennessee (562), North Carolina (922), Iowa (376), Arizona (826), Missouri (489), and Wisconsin (559) in descending order. Thus, the tweets numbers are unbalanced for each state, taking average is essential when calculating the frequency of keywords.

For the part of data visualization, we created a word cloud that shows the twitter trending words in each of the 24 states we mentioned before. The technique is to tokenize each tweet, and get the informative keywords by deleting symbols, url links, and some stopwords. Then, creating a DTM to get the appearing frequency for each term. Thus, popular trending keywords can be recognized with a high frequency. For better visualization and interactive purposes, we made a Shiny APP that customizes the word cloud based on the prefered maximum frequency and prefered maximum number of words to be shown in one word cloud entered by users. For example, in Figure 1, if a user inputs “New York” for state, 20 for maximum frequency and 40 for maximum number of words, the Shiny APP will pop up a word cloud, which includes a maximum of 40 words that appeared at least 20 times in our New York dataset. Below is a demonstration of our Shiny APP.

 <img width="361" alt="Picture1" src="https://github.com/jr-liu/Twitter-COVID-19-Sentiment-Analysis/assets/144048796/09d147e3-15d1-49f4-b8a4-a3ab1ca383ab">

In order to explore the hypothesis, we also used a LDA model to do a topic clustering based on the whole 53,213 tweets we collected over the US on May 5th, 2020. By doing this, we generated 8 related topics. In each of the topics, the top ten words are listed in Figure 2 to describe what the topic is about. The first word in each column is the most probable word that belongs to that topic.

In conclusion, topic 1 represents topic related to “reopen”, topic 2 represents topic related to “economy”, topic 3 represents topic related to “prevent illness”,  topic 4 represents topic related to “policies”,  topic 5 represents topic related to “medical”,  topic 6 represents topic related to “treatment”,  topic 7 represents topic related to “crime & stay home”,  topic 8 represents topic related to “small business”.

![Picture2](https://github.com/jr-liu/Twitter-COVID-19-Sentiment-Analysis/assets/144048796/1f352916-aeb5-480d-8cea-96d6d3951ac5)

To make the relationship between each topic and new confirmed cases of COVID-19 more clear, we also draw a scatterplot showing in Figure 3. First, we calculated the frequency of each word by dividing the tweets containing this word posted in this state by the total tweets we collected in this state. To calculate the frequency of each topic, we summed up the frequencies of those words in this topic. In this graph, we have different colors to represent different topics. The y-axis represents the frequency of the topic, and the x-axis represents the new confirmed cases on May 5th, 2020. It is obvious that each state represents a different new confirmed case on May 5th.

## Result
From Figure 3, we found that some topics have relatively strong relationships with the new confirmed cases like topic 5 and topic 7. While some topics have relatively non-obvious relationships with the new confirmed cases like topic 4, which is basically a straight line no matter how the new confirmed cases changed. 

<img width="452" alt="Picture3" src="https://github.com/jr-liu/Twitter-COVID-19-Sentiment-Analysis/assets/144048796/3f3dd270-974b-4d1f-be17-fef8ae43fabf">

To have a more strong statistical support, we built several multiple linear regression models to further study the relationship between the topics and new confirmed cases.

Showing in Figure 4, we built a multiple linear regression model with the log of new confirmed cases (“positiveIncrease”) on May 5th as the response variable, and the frequency of eight topics in each state as the dependent variables. The data of new confirmed cases on May 5th for each state is obtained from the website “covidtracking”. From the p-values, it confirmed our previous hypothesis that some topics are not really related to the confirmed cases. In details, topic 4 has a p-value of 0.9262, which means that it is highly unrelated to the new confirmed cases. Besides, some topics have relatively strong relationships with the new confirmed cases. For example, topic 5 has a p-value as 0.0412, which shows its significance. To further study the related topics, we chose two topics with the smallest p-value, topic 5 and topic 7, to build another multiple linear regression model.

![Picture4](https://github.com/jr-liu/Twitter-COVID-19-Sentiment-Analysis/assets/144048796/6018e04b-1a87-4962-93a4-433564ffa736)

In Figure 5, we built another model only with topic 5 and topic 7 as the dependent variables. Showing in the summary, both the p-values for topic 7 and the interaction between topic 5 and topic 7 are below 0.05, which means that topic 5 and topic 7 are significant. Also, the coefficients of both the topic 5 and topic 7 are positive, therefore, we could infer they have a positive relationship with the new confirmed cases. Besides, the adjusted R-squared is 0.4292, which can be interpreted that 42.92% of the variation in new confirmed cases is explained by topic 5 and topic 7. When we looked back to topic 5 and topic 7, we found that topic 5 is about healthcare, and topic 7 is about crime & stay home. Thus, these two topics on twitter have a certain relationship with the coronavirus data.

![Picture5](https://github.com/jr-liu/Twitter-COVID-19-Sentiment-Analysis/assets/144048796/c9775868-0691-4c7a-8bbb-b0af7c5439fe)

## Conclusion
In conclusion, in order to test the hypothesis that there exists a potential relationship between tweets topics on COVID-19 and daily new confirmed cases in different states, we created a scatter plot with new confirmed cases as the x-axis and word frequency as the y-axis, and then generated multiple linear regression models. We found that topic 5 on healthcare and topic 7 on crime & stay home are obviously more correlated with the new confirmed cases on May 5th, 2020. In addition to our findings, we also have a Shiny APP that displays the most frequently appeared words in twitter via a word cloud in each state.
However, in our research, there are several limitations and areas for improvement. First, we could not access data from previous days in rtweet. Therefore, our study is biased toward the data on May 5th, 2020. In this case, it may not reflect the relationship between tweets topics and new confirmed cases in the long run. For future studies, we may want to collect data of at least past 30 days to reduce bias on time. Second, since many users of Twitter do not release their location information in their tweets, we can only focus our study on tweets that have their location information disclosed. This may also cause bias in our dataset. Additionally, only tweets that contain key words “coronavirus” and “covid” were collected while some tweets also related to coronavirus may not specifically mention these two key words. Thus, our dataset is biased toward tweets with “coronavirus” and “covid”. In the future, we may need to expand the keywords list to avoid bias.


