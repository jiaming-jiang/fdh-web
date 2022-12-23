{% include head.html %}
![Beer Banner](./assets/img/beer_banner_extended.jpg "beer banner")

## <a id="section_intro">Introduction</a>
### Research questions

## <a id="section_dataset">Dataset</a>
We use the dataset provided by BeerAdvocate at this [link](https://drive.google.com/drive/folders/1Wz6D2FM25ydFw_-41I9uTwG9uNsN4TCF).

## <a id="section_clustering_approach">The Approach to the Soft-Clustering of Users</a>

### **Conformist:** 
A user which rates closely to the average rating of the beers.

<details>
  <summary>Show more details</summary>
  
  A user   $u$  is a conformist if he/she has a high conformism score defined as follows:
  $$CFM_u = \frac{1}{|B_u|}\sum_{b \in B_u}(\frac{r_{u,b}-\overline{r_b}}{\sigma_b})^2$$
  where $B_u$ is the set of beers the user $u$ has rated, $r_{u,b}$ is the rating given by user $u$ to beer $b$, $\sigma_b$ is the standard deviation of the ratings of the beer $b$, and $\overline{r_b}$ is the average rating of beer $b$.<br/>
  The metric is high if the user, on average, rates beers close to the average rating they get. This metric, aside from classifying users, could be an indicator of hearding effect if users from a particular region (a US state) have a high conformism score on average.
</details>

<br/>

### **Expert-like:** 
A user which rates closely to the reference expert rating.

<details>
  <summary>Show more details</summary>

  A user $u$ is an expert-like if he/she has a high score similarity with our reference for expert rating. The score is defined as follows:
  $$EXP_u = \frac{1}{\frac{1}{|B_u|}\sum_{b \in B_u}(\frac{r_{u,b}-ref_b}{\sigma_b})^2}$$
  where $r_{u,b}$ is the rating given by user $u$ to beer $b$, $\sigma_b$ is the standard deviation of the ratings of the beer $b$, and $ref_b$ is a reference of objective rating of a beer $b$. The reference can be either the bros score in the range $[0,5]$, or the BA score scaled down to the range $[0,5]$ (originally in $[0, 100]$). Since the BA score is more abundant (94.5% of beers rated have a BA score), we may be using it as a reference. Both scores are objective ratings provided by administrators of beeradvocate.com who we assume adhere to the guidelines of objective beer reviewing.<br/>
  The score is large if, on average, the user rates beers close to the reference score. This metric is our best shot at detecting senior beer raters and experts active on the website based solely on the available data.
</details>

<br/>

### **Explorator:**
A user which rates beers that have only a few ratings.

<details>
  <summary>Show more details</summary>
  
  A user $u$ is an explorator if he/she has a high adventurer score defined as follows where $U_{10}(b)$ is the set of at most 10 users that first rated the beer $b$:
  $$XPL_u = \sum_{b \in B_u} \mathbb{1} [u \in U_{10}(b)]$$
  This metric is larger for users that try out new beers that have not been in the spotlight (that is why they are explorators), and is low for users that almost always rate beers that have been already been rated many times before (in our case 10 times at least). This score provides us with information about which users contribute to enriching the experience on the website, either because they rate beers that do not get much attention, or because they "introduce" new beers on the website by being the first people to rate those beers.
</details>

<br/>

### **Adventurer:**
A user which rates a certain amount a poorly rated beers.

<details>
  <summary>Show more details</summary>
  
  A user 
  $u$ is an adventurer if he/she often rates a beer $b$  that has a low score at time $t_{u,b}$ at which he/she rates it, measured as follows where $T$ is a threshold to be determined empirically (see next section):
  $$ADV_u = \sum_{b \in B_u} \mathbb{1} [r_b(t_{u,b}) < T]$$
  This metric is higher for users that try out beers that have a bad rating. We want to know if the users frequenting the website are generally reluctant or willing to try out beers with bad ratings.
</details>

</br>

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
## <a id="section_reviews_ratings">Number of Ratings/Reviews and User Categories</a>

In this section, we uncover the rating/reviews tendencies of the four categories.

Let us start by  

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

{% include time_analysis_new_user_count.html %}

Considering the total yearly increments of new users belonging to all categories combined to be a measure of website popularity, one can grasp from the plot above that the website rapidly grows popular over the years to reach a peak of popularity in 2014 before dropping down in subsequent years. Additionally, the main categories  (ADV, CFM and XPL) contribute differently but consistently to these increments overtime. We notice a dominance of the explorers (XPL) in general except in 2012 and 2013 where the adventurers (ADV) and conformists (CFM) have the majority respectively. So far, we can already deduce the following:
* In most years, the website attracts more explorers (XPL) than any other category. As a quick reminder, those are the users whom around 1 in 5 beers they rate are beers newly introduced to the website since they have at most 10 ratings at the time these users rated them. These users have consistently contributed the most to the popularity of the website, or equivalently, the website has consistently attracted this kind of users most over the years. It is therefore no secret that beeradvocate.com has been consistently is a good website for users willing to explore new beers.
* Even if the explorers (XPL) dominate, the website consistently attracts a decent variety of users between conformists (CFM) and adventurers (ADV). This is emphasized by the fact that, even though we perform soft-clustering of users so each user can belong to more than one category, there is no major overlap between the three categories as shown by the Venn diagram of section "Categories' Overlap" so the trends are almost independent.
* The website has known a big surge in its number of users in 2014, which is the period highlighted, more than any other year. This surge is lead by the explorers (XPL).

We take our analysis a step further to understand the reason behind the surge in 2014. We plot the yearly contribution per category to the percentage of new classified users below:

{% include time_analysis_cat_perc_new_users.html %}

We naturally expect the largest explorers to dominate in the first few years as the website gets populated by new beers and there is still much to explore. On the other hand, on this plot, we can clearly see that a significant contribution of adventurers (ADV) has started early on and has been subject to little variance over the years. We can deduce, assuming our categories describe a broad enough spectrum of possible high level user classifications that may be of interest to the administrators, that beeradvocate.com has had a more or less consistent base of adventurers (ADV) joining the website. This is good news as one may wonder if the "labeled" not so good beers would ever have a chance to be tried by the users. The answer is yes they do, and consistently.

Let us focus on the highlighted period corresponding to year 2014. We clearly see a decrease in the share of conformists (CFM), a quasi stagnation of the share of adventurers (ADV) but a quick increase in the share of explorers (XPL). With the aim of uncovering the reason behind the 2014 surge of popularity, we again take our analysis a step further and plot the monthly increments per category in the numbers of new users in 2014 below. The highlighted period corresponds to that between and including the months of May to July.

{% include time_analysis_cat_perc_new_users_2014.html %}

In the highlighted period, the sharpe increase in the increment of the explorers (XPL) stands out since it doubles in one month and almost triples in two months, with respect to the month of May. However, prior to the month of May in the same year, the contributions of the three major categories are rather comparable. 

Accounting for the fact that we study the users from english speaking countries, 96% of them being from the USA since the website is most popular there, we were able to point out one major event that could explain this surge and that happened in the same period, around mid-May 2014, that is <a href=" https://www.craftbeer.com/news/american-craft-beer-weekz">American Craft Beer Week</a>. To sum it up, this is an event in which beer lovers get to explore and try new beers out from a variety of breweries. We believe that such an event, if not that event in particular, may have contributed to this surge in explorers (XPL), which in turn dominate the yearly increment of new classified users in 2014, which in turn contribute the most to the popularity of the website. This is another result that emphasizes on how much beeradvocate.com is attractive to people willing to explore new beers.

## <a id="section_takeaways">Takeaways</a>

TODO

