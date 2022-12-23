{% include head.html %}
![Beer Banner](./assets/img/beer_banner_extended.jpg "beer banner")

## <a id="section_intro">Introduction</a>
In this project, we soft-cluster the users of [BeerAdvocate](https://beeradvocate.com) within a set of predefined categories. Based on that, we analyse how attractive the website is for each category over time. Additionally, we uncover the trends of these categories and build interesting user personas for the website's administrators. By the end, we come up with a few conclusions about what is done right in [BeerAdvocate](https://beeradvocate.com) and what could be improved in order to make the website more attractive to the beer lovers community. We also provide a more general result in the form of the conclusion that natural soft-clustering<sup>1</sup> of users can help uncover user groups' behaviors which provides room for improvement in many businesses, including those that heavily rely on recommender systems and providing custom user experiences. We have available to us a dataset of [BeerAdvocate](https://beeradvocate.com) data spanning between 1996 and 2017.

<i><sup>1</sup>natural soft-clustering: this approach is "cluster together users that satisfy a human-interpretable condition" rather than "cluster together users that are similar based on a similarity metric". Since one user can satisfy many conditions simulataneously or none at all, any user can belong to any given number of categories including none at all. That is what we mean by natural soft-clustering of users.</i>

## <a id="section_dataset">Dataset</a>
We use the [dataset](https://drive.google.com/drive/folders/1Wz6D2FM25ydFw_-41I9uTwG9uNsN4TCF) provided by the teaching team of "CS-401:Applied Data Analysis" at EPFL for the year 2022.

## <a id="section_clustering_approach">The Approach to Soft-Clustering the Users</a>

We use a score-based approach to soft-cluster users using scores that are easily interpretable to humans. All of our scores satisfy that the larger they are the more they emphasize that the user may belong to the given category.

### **Conformists Category** 
A user who rates beers close to their average rating, meaning that on average this user deviates little from the average opinion on the beers he/she rates.

This category may be regarded as an indicator of hearding effect where users tend, on average, to stick to the average opinion when rating beers even if the average opinion may be unfair towards a beer.

<details>
  <summary>Show more</summary>
  
  A user $u$ is a conformist if he/she has a high conformism score that measures how much the user sticks with the average opinion on the beers he/she rates. The score is defined as follows:

  $$CFM_u = -\frac{1}{B_u} \sum_{b \in B_u} (\frac{r_{u,b}-\overline{r_b}}{\sigma_b})^2$$
  
  $B_u$ is the set of beers the user $u$ has rated.
  
  $r_{u,b}$ is the rating given by user $u$ to beer $b$.
  
  $\sigma_b$ is the standard deviation of the ratings of the beer $b$.

  $\overline{r_b}$ is the average rating of beer $b$.
   
</details>

<br>

### **EXP Users Category** 
A user that rates beers close the BA score that is displayed on the website for most beers. According to [BeerAdvocate](https://beeradvocate.com) administrators, the BA score is a reference score displayed on the website to give the users and idea about how a beer ranks among the beers of the same style.

<i>Note that 94.5% of rated beers in the data available to us have a BA score.</i>

For lack of a better name, we will reference these users as the EXP users refering to a somewhat "structured" way of rating beers that may indicate being influenced by the BA score displayed on the website.

<details>
  <summary>Show more</summary>
  A user $u$ is an EXP user if he/she rates, on average, beers close to their BA score scaled down to the range [0, 5] using the following piecewise linear mapping based on the <a href="https://www.beeradvocate.com/community/threads/beeradvocate-ratings-explained.184726/">BA score range and meaning revealed by the website's administrators</a>, to which we believe this mapping corresponds best:
  
 {world class : 95 - 100 mapped to ]4.75; 5], outstanding : 90 - 94  mapped to ]4.5; 4.75], very good : 85 - 89  mapped to ]4; 4.5], good : 80 - 84  mapped to ]3.5; 4], okay : 70 - 79  mapped to ]3; 3.5], poor : 60 - 69 mapped to ]2; 3], awful : <= 59 mapped to ]0; 2]}.
  
  The score is defined as follows:
  
  $$EXP_u = -\frac{1}{|B_u|} \sum_{b \in B_u} (\frac{r_{u,b}-BA_b}{\sigma_b})^2$$

  $B_u$ is the set of beers rated by user $u$.
  
  $r_{u,b}$ is the rating given by user $u$ to beer $b$.
  
  $\sigma_b$ is the standard deviation of the ratings of the beer $b$.
  
  $BA_b$ is the BA score a beer $b$. 
</details>

<br>

### **Explorers Category**
A user that rates beers having a few ratings only if not none at all. In the latter case, we consider that user  to be the one who added the beer to the website. (yes, [BeerAdvocate](https://beeradvocate.com) allows any user to add new beers to its database.)

These users therefore shed light (positively or negatively) on unpopular beers by rating them, and some of them help populate the website with new beers which enriches the user experience on the website. 

<details>
  <summary>Show more</summary>
  This score provides us with information about which users contribute to enriching the experience on the website, either because they rate beers that do not get much attention, or because they "introduce" new beers on the website by being the first people to rate those beers.
  
  A user $u$ is an explorer if, often enough, among the beers he rated, he/she was among the first 10 users to rate that beer. The score is therefore: 

  $$XPL_u = \sum_{b \in B_u} \mathbb{1} [u \in U_{10}(b)]$$

  $B_u$ is the set of beers rated by user $u$.

  $U_{10}(b)$ is the set of at most 10 users that first rated the beer $b$.
  
</details>

<br>

### **Adventurer Category**
A user that rates often enough beers that are slightly okay at best. 

These users are willing to risk trying out bad beers. We are interested in such users because they may contribute to the visibility of underated beers in the website and so indirectly to the user experience.

<details>
  <summary>Show more</summary>
  
  A user $u$ is an adventurer if he/she often enough risks trying out beers that have a bad rating at the time at which they rate them (less than 3.25/5, knowing that the average is at $\approx$ 3.97/5). The corresponding score is the following:

  $$ADV_u = \sum_{b \in B_u} \mathbb{1} [\overline{r}_{u,b}(t_{u,b}) < T]$$

  $B_u$ is the set of beers rated by user $u$.

  Rating $\overline{r}_b(t_{u,b})$ is the average rating of beer $b$ at the time $t_{u,b}$ at which user $u$ rates beer $b$.

  $T=3.25$ is a cut-off determined empirically based on the data.
</details>

<br>

## <a id="section_clustering">User Clustering</a>

In order to choose adequate thresholds to classify the users based on the scores defined in the previous section, we look at the distributions of these scores. The figures below plot those distributions in real and logarithmic scales:

![Alt text](./assets/svg/adv_distribution.svg)

The adventurer score has a heavy-tail distributed. By the interpretability of this score, a cut-off or threshold at 0.2 classifies as adventurers the users for which at least 20% of the beers they rate have a slightly okay at best rating at the moment at which they rate them. We use this threshold that corresponds to the $90^{th}$ percentile of the ADV score distribution.
    
![Alt text](./assets/svg/cfm_distribution.svg) 

The conformist score also has a heavy-tail distributed. Here again, we choose the threshhold that corresponds to the $90^{th}$ percentile that is -0.35, which by the interpretability of this score means that we classify as conformists the users who deviate on average by less than 0.35 from the average opinion.
    
![Alt text](./assets/svg/exp_distribution.svg) 

The EXP score looks more like a skewed gaussian, but it is not since it is heavy-tailed. Again by the interpretability of this score, a threshold at -0.2 classifies users that deviate on average by less than 0.2 from the displayed BA score as EXP users.
   
![Alt text](./assets/svg/xpl_distribution.svg)  

The explorer score also follows a heavy-tailed distribution. To reiterate, by the interpretability of this score, given a threshold of approximately 0.2, we classify as explorers the users that figure in the first 10 raters of at least 20% of the beers they rate.

### <a id="section_overlap">To What Extent Do the Categories Overlap?</a>

Since we do soft clustering, users have a score for each category. But how users who meet the threshold score to be in multiple categories ? We used a simple Venn diagram to visualize the overlap of categories. The Venn diagram show the number of users for each categories and the number of users who are in the intersection of multiple categories.


![Alt text](./assets/svg/venn_diagram_categories.svg) 

As we can see, there are very few overlaps between explorers, conformists and adventurers. There are some users with two categories, but almost no user with all 3 categories. This suggests that the categories that we defined target different kind of users.

# <a id="section_story">The Story Overview</a>
In the following sections, we will study the categories defined previously on many levels: beer style preferences, locations, ratings, reviews...etc to extract as much information about these categories as possible to build relevant personas that can be leveraged by the administrators to improve the UX of their website.

## <a id="section_ratings_reviews">How Do the Number of Ratings and Reviews Characterize the Categories?</a>

In this section, we highlight the ratings and reviews tendencies of the four categories: XPL, ADV, CFM and EXP.

In order to know if the number of ratings characterizes in some way each category, we plot the likelihood of a user belonging to a category as a function of the range in which his/her number of ratings falls. Each range corresponds to an inter-quantile range of around 25% of the density of the number of ratings of all the studied users. As a quick reminder, those are the users from english speaking countries having at least 5 ratings. The figure below shows the result.

It is worth pointing out that we only consider 99% of the density as the remaining 1% contains only super users which have many folds more ratings than normal users. Since there are very few super users, their results may be non-conclusive because they may be non-representative of super users overall that is why treat them as outliers and discard them here.

{% include ratings_and_cathegory_proba_analysis.html %}

We notice the following trends:
* Adventurers (ADV) are more likely to have fewer ratings (100 ratings at most), but could also have more ratings. The probability is however almost halved for subsequent ranges where the number of ratings is larger than 100.
* Conformists (CFM) are more likely to have so many ratings (more than 500 ratings).
* Explorers (XPL) are almost as likely to have very few or so many ratings (more than 500 ratings).
* EXP users (EXP) mostly have very few ratings (10 at most) and become quickly significantly less likely as the number of ratings increases.

We can therefore deduce the following about:
* Adventurers (ADV): the more ratings a user has, the less likely he/she tends to try out not so good beers. This is not explained by the unwillingness of users with many ratings to try bad beers, but rather by the fact that the average beer rating is quite high ($\approx$ 3.97/5 overall) so users who have a lot of ratings end up rating beers that on average have a good score which reduces the percentage of bad beers they rate and thus throws them off being classified as adventurers.
* Conformists (CFM): the users who rate on average close to the average rating of a beer tend to have many ratings, unabiding by what one could expect.
* EXP users (EXP): the users that rate beers close (0.2 average deviation from the true value) to the piecewise linear mapping of the BA score to the range [0, 5] do so unintentionally since they tend to have very few ratings. We can already deduce that the display of the BA score on the website has no significant influence on the way the users rate the beers.
* Explorers (XPL): the users who tend to rate newly introduced beers or introduce new beers are more likely to have many or very few ratings.

In the same spirit, we filter the users having at least one review and repeat the process. The figure below shows the result:

{% include reviews_and_cathegory_proba_analysis.html %}

This time, we highlight the most important trend that is that the explorers (XPL) are overall the most likely to provide a review for the beers they rate.

## <a id="section_countries">Are There More Users from a Specific Country for Certain Categories?</a>

For our analysis, we took only users from the USA, Canada, UK and Australia. While most users in [BeerAdvocate](https://beeradvocate.com) come from the USA. Is the distribution of country the same for all categories ? The following interactive plot (you can select a category) shows the distributions of countries for the users for all users (ALL), conformist (CFM), explorer (XPL), adventurer (ADV) and EXP users.

{% include top_countries_pie_chart.html %}

We can conclude from the plot that conformists (CFM) and EXP users (EXP) have the same country distribution as the distribution of all users regardless of categories.  
However, we see a different distribution for explorers (XPL) and adventurers (ADV), with a higher propotion of users from these categories comming from Canada, England and Australia. This could potentially be due to a tendency for these users to rate beers from their own country or region more often. Indeed, since Canadian, English and Australian beers are more rare in [BeerAdvocate](https://beeradvocate.com), if the users from this countries rate the beers from their country more, there are more likely to rate beers with few or bad ratings.  

The pie chart shows only the distribution of categories at the country level. However, we need to zoom in the USA to have a bigger picture since it's where most users in [BeerAdvocate](https://beeradvocate.com) come from. In order to get a clearer look on this disparities per location we can decompose the USA in the diffrent existing states. The following plot show the distribution of US state by categories.

{% include 10highestLocationPerCategories.html %}

 Clearly there is a high disparity of the percentages of users per location. CFM and EXP varies between twice and one time the overall categories value (10% and 3% of the user respectively). However the XPL and ADV categories have higher disparities with the Austria England and Canada having extremely high percentages. 60% of Austrian users are explorer this is more than 4 times the overall percentage of 13%. The concentration of XPL in these countries can be explained by the extremly low number of selected users in these countries. Austria 229 Canada 1593 and England 329. This is in fact so low that when a user rate a local beer there is a high chance that this beer has been rated less than 10 times in the meantime classifying them as XPL. In order to explains the ADV spike in this countries we can look at the average rating of the beers that these 3 countries's users rate indeed Austria is the countrie with the users rating the beers with the lowest average rating (3.64), England is the second (3.70) and Canada is the 4th (3.75). This explain why Australian, English and Canadian users have a tendancy to rate beers with low average rating and be classified as ADV.
  
## <a id="section_beers">Do Different Categories Have Different Beer Preferences?</a>

### Do users belonging to different categories on BeerAdvocate have similar preferences for the region of origin of beers ?

Some region in the world have a high reputation in terms of brewing beers. We will first analysis to which country where the beer was brewed the ratings of users go to. The following interactive plot (you can click on categories) show, for each category, the percentage of rating that go to each location (US state and countries) ranked by highest percentage first. 
  
{% include beer_ratings_by_location.html %}
  
We can see a similar pattern for all categories: they rate beers from where they come from. Indeed, we see that most ratings go to the most populous states of the US. However, by selecting adventurer (ADV) and overall (ALL) only on the plot, we see that adventurers, while still rating beers from California a lot, have a different behaviour to the general rater. By selecting only 'adventurer' we see that they also rate beers from other countries a lot: beers from Canada, Germany, Belgium and England for example.  
While Canada and England may not be surprising since a lot of adventurers come from these two countries. The other countries are more suprising since there is no user from Belgium and Germany from the users we considered. Beers from Belgium which are very famous have a high percent of ratings across all categories, however 5% of ratings from adventurer users go to beers brewed in Germany, while only 2% of ratings from users in general go to Germany. It seems like adventurers are more likely to rate beers from other countries than theirs compared to the other categories. 
This behaviour can compared to explorers who also rates a lot of beers comming from Canada (we see that a lot of explorers are Canadian), but they don't rate beer from Germany as often as explorers.

We see that adventurers (ADV) don't rate the same kind of beers compared to the other user categories. However, do they have similar likings in terms of beer provenance? The following interactive plot (you can click on a category) shows the ratio between the average rating for each location compared to the average rating of a category of user. If the average rating for a location is above 1 (the dashed line) it means that users from the category selected like more beers from that location compared to the other beers they rated.

{% include beer_location_most_liked.html %}
  
By selecting all categories, we see that conformists (CFM), EXP users and explorers (XPL) mostly agree on the best beer provenance. Moreover, they all have a similar liking compared to users in general (ALL).
However, adventurers have different preferences. By clicking on adventurers (ADV) and overall (ALL) on the plot, we see for example that they like more beers from Belgium compared to the general users (ALL). There is also is a huge difference for beers from Scotland. Overall, they have very different opinions compared to the other categories.

### Do users from different categories like the same beer styles ? 

Now that we concluded that conformists (CFM), EXP users and explorers (XPL) have similar behaviour in terms of ratings of beers towards location of origin of beers, and that adventurers (ADV) have a different behaviour. We can see our conclusion also applies to beer styles, which is also a very opinionated topic. 
The following interactive plot (you can click on categories) is similar to the first plot for location of origin of beers. It displays  for each category, the percentage of rating that go to each bear style ranked by highest percentage first. 

{% include top_beer_styles.html %}
  
When we select the users overall (ALL) and conformist users (CFM) on the plot, we see that these two categories are very similar in terms of percent of ratings for beer styles.
However, by selecting ALL again and EXP or explorer users (XPL) we see a similar ranking, but the distribution is not the same. This motivates the idea that conformists are the closest user to the lambda user.  
Finally, we see a very different ranking and distribution for the adventurers. It seems like they rate very different style of beers compared to the other users.

We then compare the likings of each categories in terms of beer style to see if a beer style is more liked by some categories. The following interactive plot (you can click on a category) shows the ratio between the average rating for each beer style compared to the average rating of a category of user. If the average rating for a beer style is above 1 (the dashed line) it means that users from the category selected like more beers from that beer style compared to the other beers they rated.

{% include top_liked_beer_styles.html %}

Conformist, explorers and expert-like rate style of beers similarly compared to their average rating. Additionaly, it seems like they all agree that "Geuze", a belgian style of beer, is the best style overall.  
The only category that doesn't agree with this consensus is the adventurer category: The Gueuzes are only ranked 11 in their ranking. Their prefered style is quadrupel, which is also belgian and according to the webiste, it's not a very common type of beer in the USA (where most users of [BeerAdvocate](https://beeradvocate.com) come from). Another interesting result is that, while adventurers are users who rate bears with bad average rating, they tend to rate quadrupel beers a lot according to the plot before showing the percent of ratings. It reveals that, even though adventurers rate a lot of bad beers, they also rate a lot of beers they like.

The following plot show what are the most disliked style of beers for adventurer users (ADV). It's the same kind of plot as above but focuses only adventurers and ranks the beer style by worst normalized average rating first.

{% include adventurer_least_liked_beer_styles.html %}

We notice that the American Adjunct Lager is present, while being the beer they rate the most. We can also notice other beers that they rate a lot while considering it not good (eg. Euro Pale Lager), suggesting that they rate a lot of beers that they don't like. However, this oudl not be true to conclude that they only rate beers they don't like. For example, their second most rated beer is American IPA, which is very popular for all users, and they don't seem to dislike them. This suggests that there behaviour is not orthogonal to the general user.

## <a id="section_sent_analysis">How Does Reviews' Sentiment Analysis Compare to Ratings for each Category?</a>

The different user categories don't have the same average rating: $\approx$3.92/5 for conformists (CFM), $\approx$4.39 for EXP users, $\approx$3.8 for explorers (XPL) and $\approx$3.45 for adventurers (ADV). The difference for ADV is due to the fact that they rate badly rate beers by definition. But how is this difference in average rating reflected in the sentiment of the reviews?
We performed a sentiment analysis on the reviews of the users of each category and a sample of users took uniformly regardless of their category. The following plot shows the average "compound sentiment" for the beer reviews of each category using Vader sentiment analyser. The compound sentiment is considered a postive sentiment if its values is above 0.05, neutral if its between 0.05 and -0.05 and negative if it is below -0.05.  

![Alt text](./assets/svg/sentiment_scores.svg)
  
Sentiments of review are mostly very positive for each category. The average sentiment is significantly above the threshold for positive sentiment. Conformist, explorers have the same average sentiment as the general user. However, expert-like are slighly more postive and adventurers slightly more negative compared to the general user.
The disparity between the categories can be explained by they average rating, which is different as we mentioned above. However, the difference in average rating between categories is not fully translated into the sentiment analysis. Although the difference of average rating is huge between adventurers and the general user ($\approx$3.45 vs $\approx$3.89 for users in general), the difference of average sentiment is not that noticeable. This suggests some kind of "regression to the mean" phenomenon: Adventurers may have some reviews which may be more negative compared to the average review, the average sentiment of an adventurer seems to tend towards the average sentiment of beer reviews in general for [BeerAdvocate](https://beeradvocate.com) though, which seems to be quite positive.


## <a id="section_personas">Categories' Personas</a>
### Explorers (XPL)
TODO
### Conformists (CFM)
TODO
### Adventurers (ADV)
TODO
### Reference-like (EXP)
TODO

<!-- Farouk -->
## <a id="section_time_analysis">What Categories Does the Website Attract Most Overtime?</a>
In this section, we she light on the attractiveness of [BeerAdvocate](https://beeradvocate.com) for each user category overtime. Firstly, we look at the yearly increments in the number of new users broken down by category. The figure below shows those increments for the period spanned by the data:

{% include time_analysis_new_user_count.html %}

Considering the total yearly increments of new users belonging to all categories combined to be a measure of website popularity, one can grasp from the plot above that the website rapidly grows popular over the years to reach a peak of popularity in 2014 before dropping down in subsequent years. Additionally, the main categories  (ADV, CFM and XPL) contribute differently but consistently to these increments overtime. We notice a dominance of the explorers (XPL) in general except in 2012 and 2013 where the adventurers (ADV) and conformists (CFM) have the majority respectively. So far, we can already deduce the following:
* In most years, the website attracts more explorers (XPL) than any other category. As a quick reminder, those are the users whom around 1 in 5 beers they rate are beers newly introduced to the website since they have at most 10 ratings at the time these users rated them. These users have consistently contributed the most to the popularity of the website, or equivalently, the website has consistently attracted this kind of users most over the years. It is therefore no secret that [BeerAdvocate](https://beeradvocate.com) has been consistently is a good website for users willing to explore new beers.
* Even if the explorers (XPL) dominate, the website consistently attracts a decent variety of users between conformists (CFM) and adventurers (ADV). This is emphasized by the fact that, even though we perform soft-clustering of users so each user can belong to more than one category, there is no major overlap between the three categories as shown by the Venn diagram of section "Categories' Overlap" so the trends are almost independent.
* The website has known a big surge in its number of users in 2014, which is the period highlighted, more than any other year. This surge is lead by the explorers (XPL).

We take our analysis a step further to understand the reason behind the surge in 2014. We plot the yearly contribution per category to the percentage of new classified users below:

{% include time_analysis_cat_perc_new_users.html %}

We naturally expect the largest explorers to dominate in the first few years as the website gets populated by new beers and there is still much to explore. On the other hand, on this plot, we can clearly see that a significant contribution of adventurers (ADV) has started early on and has been subject to little variance over the years. We can deduce, assuming our categories describe a broad enough spectrum of possible high level user classifications that may be of interest to the administrators, that [BeerAdvocate](https://beeradvocate.com) has had a more or less consistent base of adventurers (ADV) joining the website. This is good news as one may wonder if the "labeled" not so good beers would ever have a chance to be tried by the users. The answer is yes they do, and consistently.

Let us focus on the highlighted period corresponding to year 2014. We clearly see a decrease in the share of conformists (CFM), a quasi stagnation of the share of adventurers (ADV) but a quick increase in the share of explorers (XPL). With the aim of uncovering the reason behind the 2014 surge of popularity, we again take our analysis a step further and plot the monthly increments per category in the numbers of new users in 2014 below. The highlighted period corresponds to that between and including the months of May to July.

{% include time_analysis_cat_perc_new_users_2014.html %}

In the highlighted period, the sharpe increase in the increment of the explorers (XPL) stands out since it doubles in one month and almost triples in two months, with respect to the month of May. However, prior to the month of May in the same year, the contributions of the three major categories are rather comparable. 

Accounting for the fact that we study the users from english speaking countries, 96% of them being from the USA since the website is most popular there, we were able to point out one major event that could explain this surge and that happened in the same period, around mid-May 2014, that is <a href=" https://www.craftbeer.com/news/american-craft-beer-week">American Craft Beer Week</a>. To sum it up, this is an event in which beer lovers get to explore and try new beers out from a variety of breweries. We believe that such an event, if not that event in particular, may have contributed to this surge in explorers (XPL), which in turn dominate the yearly increment of new classified users in 2014, which in turn contribute the most to the popularity of the website. This is another result that emphasizes on how much [BeerAdvocate](https://beeradvocate.com) is attractive to people willing to explore new beers.

## <a id="section_takeaways">Takeaways</a>
In light of our analysis, we would like to highlight the following facts and improvements that could be made to the website:

* TODO
* TODO
* TODO
* TODO

On the other hand, this analysis is an example of a successful natural soft-clustering approach to grasp the user tendencies by category and to build personas that can be useful to improve the underlying service or platform. We believe our approach is fruitful in services that rely heavily on recommender systems where understanding and characterizing different classes of users may be helpful in predicting the best item suggestions.
