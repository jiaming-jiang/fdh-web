{% include head.html %}
![Beer Banner](./assets/img/beer_banner_extended.jpg "beer banner")

## <a id="section_intro">Introduction</a>
In this project, we soft-cluster the users of beeradvocate.com within a set of predefined categories. Based on that, we analyse how attractive the website is for each category over time. Additionally, we uncover the trends of these categories and build interesting user personas for the website's administrators. By the end, we come up with a few conclusions about what is done right in beeradvocate.com and what could be improved in order to make the website more attractive to the beer lovers community. We also provide a more general result in the form of the conclusion that natural soft-clustering<sup>1</sup> of users can help uncover user groups' behaviors which provides room for improvement in many businesses, including those that heavily rely on recommender systems and providing custom user experiences. We have available to us a dataset of beeradvocate.com data spanning between 1996 and 2017.

<i><sup>1</sup>natural soft-clustering: this approach is "cluster together users that satisfy a human-interpretable condition" rather than "cluster together users that are similar based on a similarity metric". Since one user can satisfy many conditions simulataneously or none at all, any user can belong to any given number of categories including none at all. That is what we mean by natural soft-clustering of users.</i>


### Research questions

## <a id="section_dataset">Dataset</a>
We use the [dataset](https://drive.google.com/drive/folders/1Wz6D2FM25ydFw_-41I9uTwG9uNsN4TCF) provided by the teaching team of "CS-401:Applied Data Analysis" at EPFL for the year 2022.

## <a id="section_clustering_approach">The Approach to Soft-Clustering the Users</a>

We use a score-based approach to soft-cluster users using scores that are easily interpretable to humans. All of our scores satisfy that the larger they are the more they emphasize that the user may belong to the given category.

This category may be regarded as an indicator of hearding effect where users tend, on average, to stick to the average opinion when rating beers even if the average opinion may be unfair towards a beer.

### **Conformist:** 
A user who rates beers close to their average rating, meaning that on average this user deviates little from the average opinion on the beers he/she rates.

<details>
  <summary>Show more</summary>
  
  A user $u$ is a conformist if he/she has a high conformism score that measures how much the user sticks with the average opinion on the beers he/she rates. The score is defined as follows:

  $$CFM_u = -\frac{1}{B_u} \sum_{b \in B_u} (\frac{r_{u,b}-\overline{r_b}}{\sigma_b})^2$$
  
  * $B_u$ is the set of beers the user $u$ has rated.
  * $r_{u,b}$ is the rating given by user $u$ to beer $b$
  * $\sigma_b$ is the standard deviation of the ratings of the beer $b$
  * $\overline{r_b}$ is the average rating of beer $b$.
   
</details>

<br>

### **EXP user** 
A user that rates beers close to the piecewise linear mapping of the beers' BA scores to their corresponding equivalent in the range [0, 5]. The BA score is a reference score  displayed on the website to give the users and idea about how the beer ranks among the beers of the same style, and is a value initially in the range [0, 100].

For lack of a better name, we will reference these users as the EXP users refering to a somewhat "structured" way of rating beers that may indicate being influenced by the BA score displayed on the website.

<details>
  <summary>Show more</summary>

  A user $u$ is an EXP user if he/she rates, on average, beers close to their BA score scaled down to the range [0, 5] using the following piecewise linear mapping based on the <a href="https://www.beeradvocate.com/community/threads/beeradvocate-ratings-explained.184726/">BA score range and meaning revealed by the website's administrators</a>, to which we believe this mapping corresponds best:
  
  * world class : 95 - 100 mapped to ]4.75; 5]
  * outstanding : 90 - 94  mapped to ]4.5; 4.75]
  * very good   : 85 - 89  mapped to ]4; 4.5]
  * good        : 80 - 84  mapped to ]3.5; 4]
  * okay        : 70 - 79  mapped to ]3; 3.5]
  * poor        : 60 - 69  mapped to ]2; 3]
  * awful       : <= 59    mapped to ]0; 2]
  
  The score is defined as follows:
  
  $$EXP_u = -\frac{1}{|B_u|} \sum_{b \in B_u} (\frac{r_{u,b}-ref_b}{\sigma_b})^2$$
  
  * $r_{u,b}$ is the rating given by user $u$ to beer $b$.
  * $\sigma_b$ is the standard deviation of the ratings of the beer $b$.
  * $ref_b$ is a reference of objective rating of a beer $b$. 
  
  Note that 94.5% of rated beers in the data available to us have a BA score.
</details>

<br>

### **Explorer:**
A user that rates beers having a few ratings only if not none at all. In the latter case, we consider that user  to be the one who added the beer to the website. (yes, beeradvocate.com allows any user to add new beers to its database.)

These users therefore shed light (positively or negatively) on unpopular beers by rating them, and some of them help populate the website with new beers which enriches the user experience on the website. 

<details>
  <summary>Show more</summary>
  
  A user $u$ is an explorer if, often enough, among the beers he rated, he/she was among the first 10 users to rate that beer. The score is therefore: 

  $$XPL_u = \sum_{b \in B_u} \mathbb{1} [u \in U_{10}(b)]$$

  * $U_{10}(b)$ is the set of at most 10 users that first rated the beer $b$.
  
  This score provides us with information about which users contribute to enriching the experience on the website, either because they rate beers that do not get much attention, or because they "introduce" new beers on the website by being the first people to rate those beers.
</details>

<br>

### **Adventurer:**
A user that rates often enough beers that are slightly okay at best. 

These users are willing to risk trying out bad beers. We are interested in such users because they may contribute to the visibility of underated beers in the website and so indirectly to the user experience.

<details>
  <summary>Show more</summary>
  
  A user $u$ is an adventurer if he/she often enough risks trying out beers that have a bad rating at the time at which they rate them (less than 3.25/5, knowing that the average is at $\approx$ 3.97/5). The corresponding score is the following:

  $$ADV_u = \sum_{b \in B_u} \mathbb{1} [r_b(t_{u,b}) < T]$$

  - the time $t_{u,b}$  is the time at which user $u$ rates beer $b$.
  - $T=3.25$ is a cut-off determined empirically based on the data.
</details>

<br>

## <a id="section_classification">User Classification</a>

![Alt text](./assets/svg/adv_distribution.svg)

TODO
    
![Alt text](./assets/svg/cfm_distribution.svg) 

TODO
    
![Alt text](./assets/svg/exp_distribution.svg) 

TODO
   
![Alt text](./assets/svg/xpl_distribution.svg)  

TODO

### Categories' Overlap
![Alt text](./assets/svg/venn_diagram_categories.svg)  
  
TODO

<!-- Farouk -->
## <a id="section_ratings_reviews">Number of Ratings/Reviews and User Categories</a>

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

## <a id="section_countries">Countries and User Categories</a>
{% include top_countries_pie_chart.html %}{width=100%}

TODO

## <a id="section_beers">Beers and User Categories</a>

### The Locations with the Most Rated and Most Liked Beers

{% include beer_ratings_by_location.html %}

TODO

{% include beer_location_most_liked.html %}

TODO

### The Most Rated and Most Liked Beer Styles

{% include top_beer_styles.html %}

TODO

{% include top_liked_beer_styles.html %}

TODO

{% include adventurer_least_liked_beer_styles.html %}

TODO


## <a id="section_reg_analysis">Regression analysis</a>
![Alt text](./assets/svg/adventurer_regression.svg)

TODO

## <a id="section_sent_analysis">Sentiment analysis on User Reviews</a>
![Alt text](./assets/svg/sentiment_scores.svg)

TODO

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
## <a id="section_time_analysis">Time Analysis of the Attractiveness of the Website</a>
In this section, we she light on the attractiveness of beeradvocate.com for each user category overtime. Firstly, we look at the yearly increments in the number of new users broken down by category. The figure below shows those increments for the period spanned by the data:

{% include time_analysis_new_user_count.html %}{width=100%}

Considering the total yearly increments of new users belonging to all categories combined to be a measure of website popularity, one can grasp from the plot above that the website rapidly grows popular over the years to reach a peak of popularity in 2014 before dropping down in subsequent years. Additionally, the main categories  (ADV, CFM and XPL) contribute differently but consistently to these increments overtime. We notice a dominance of the explorers (XPL) in general except in 2012 and 2013 where the adventurers (ADV) and conformists (CFM) have the majority respectively. So far, we can already deduce the following:
* In most years, the website attracts more explorers (XPL) than any other category. As a quick reminder, those are the users whom around 1 in 5 beers they rate are beers newly introduced to the website since they have at most 10 ratings at the time these users rated them. These users have consistently contributed the most to the popularity of the website, or equivalently, the website has consistently attracted this kind of users most over the years. It is therefore no secret that beeradvocate.com has been consistently is a good website for users willing to explore new beers.
* Even if the explorers (XPL) dominate, the website consistently attracts a decent variety of users between conformists (CFM) and adventurers (ADV). This is emphasized by the fact that, even though we perform soft-clustering of users so each user can belong to more than one category, there is no major overlap between the three categories as shown by the Venn diagram of section "Categories' Overlap" so the trends are almost independent.
* The website has known a big surge in its number of users in 2014, which is the period highlighted, more than any other year. This surge is lead by the explorers (XPL).

We take our analysis a step further to understand the reason behind the surge in 2014. We plot the yearly contribution per category to the percentage of new classified users below:

{% include time_analysis_cat_perc_new_users.html %}{width=100%}

We naturally expect the largest explorers to dominate in the first few years as the website gets populated by new beers and there is still much to explore. On the other hand, on this plot, we can clearly see that a significant contribution of adventurers (ADV) has started early on and has been subject to little variance over the years. We can deduce, assuming our categories describe a broad enough spectrum of possible high level user classifications that may be of interest to the administrators, that beeradvocate.com has had a more or less consistent base of adventurers (ADV) joining the website. This is good news as one may wonder if the "labeled" not so good beers would ever have a chance to be tried by the users. The answer is yes they do, and consistently.

Let us focus on the highlighted period corresponding to year 2014. We clearly see a decrease in the share of conformists (CFM), a quasi stagnation of the share of adventurers (ADV) but a quick increase in the share of explorers (XPL). With the aim of uncovering the reason behind the 2014 surge of popularity, we again take our analysis a step further and plot the monthly increments per category in the numbers of new users in 2014 below. The highlighted period corresponds to that between and including the months of May to July.

{% include time_analysis_cat_perc_new_users_2014.html %}{width=100%}

In the highlighted period, the sharpe increase in the increment of the explorers (XPL) stands out since it doubles in one month and almost triples in two months, with respect to the month of May. However, prior to the month of May in the same year, the contributions of the three major categories are rather comparable. 

Accounting for the fact that we study the users from english speaking countries, 96% of them being from the USA since the website is most popular there, we were able to point out one major event that could explain this surge and that happened in the same period, around mid-May 2014, that is <a href=" https://www.craftbeer.com/news/american-craft-beer-weekz">American Craft Beer Week</a>. To sum it up, this is an event in which beer lovers get to explore and try new beers out from a variety of breweries. We believe that such an event, if not that event in particular, may have contributed to this surge in explorers (XPL), which in turn dominate the yearly increment of new classified users in 2014, which in turn contribute the most to the popularity of the website. This is another result that emphasizes on how much beeradvocate.com is attractive to people willing to explore new beers.

## <a id="section_takeaways">Takeaways</a>

TODO

