![Beer Banner](./assets/img/beer_banner_extended.jpg "beer banner")

# [BeerAdvocate](https://www.beeradvocate.com/), what kind of raters does the website attract?
## Motivation
## Goal
## Research questions
## Data
We use the dataset provided by BeerAdvocate at this [link](https://drive.google.com/drive/folders/1Wz6D2FM25ydFw_-41I9uTwG9uNsN4TCF).
## The data analysis

### The categories
**Conformist**
<details>
  <summary>Show more details</summary>
  
A user $u$ is a conformist if he/she has a high conformism score defined as follows:

$$CFM_u = \frac{1}{|B_u|}\sum_{b \in B_u}(\frac{r_{u,b}-\overline{r_b}}{\sigma_b})^2$$

The metric is high if the user, on average, rates beers close to the average rating they get. This metric, aside from classifying users, could be an indicator of hearding effect if users from a particular region (a US state) have a high conformism score on average.
</details>

### ...
![Plot2](./assets/img/plot2.jpg "plot 2")
_<ca>  Fig1: This plot shows...  </ca>_

{% include top_10_styles.html %}
