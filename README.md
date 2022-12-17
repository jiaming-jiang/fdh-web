![Beer Banner](./assets/img/beer_banner_extended.jpg "beer banner")

# [BeerAdvocate](https://www.beeradvocate.com/), what kind of raters does the website attract?
## Motivation
## Goal
## Research questions
## Data
We use the dataset provided by BeerAdvocate at this [link](https://drive.google.com/drive/folders/1Wz6D2FM25ydFw_-41I9uTwG9uNsN4TCF).
## The data analysis

### The categories

- **Conformist**: A user which rates closely to the average rating of the beers.

<details>
  <summary>Show more details</summary>
  
A user 
$u$
is a conformist if he/she has a high conformism score defined as follows:

$$CFM_u = \frac{1}{|B_u|}\sum_{b \in B_u}(\frac{r_{u,b}-\overline{r_b}}{\sigma_b})^2$$

The metric is high if the user, on average, rates beers close to the average rating they get. This metric, aside from classifying users, could be an indicator of hearding effect if users from a particular region (a US state) have a high conformism score on average.
</details>


- **Expert-like**: A user which

<details>
  <summary>Show more details</summary>

A user 
$u$
is an expert-like if he/she has a high score similarity with our reference for expert rating. The score is defined as follows:
$$EXP_u = \frac{1}{\frac{1}{|B_u|}\sum_{b \in B_u}(\frac{r_{u,b}-ref_b}{\sigma_b})^2}$$

The score is large if, on average, the user rates beers close to the reference score. This metric is our best shot at detecting senior beer raters and experts active on the website based solely on the available data.
</details>

### ...
![Plot2](./assets/img/plot2.jpg "plot 2")
_<ca>  Fig1: This plot shows...  </ca>_

{% include top_10_styles.html %}
