---
layout:     post
title:      "Week 1 - Introducing Recommender Systems"
subtitle:   "Introduction to Recommender Systems: Non-Personalized and Content-Based"
date:       2017-11-27
author:     "Zexi"
header-img: "img/post-bg-digital-native.jpg"
catalog: true
tags:
    - Recommender Systems
---

## Introducing Recommender Systems

### MovieLens Tour

* Recommends movies to users
* Publishes data sets (which we’ll be using)
* Available at https://movielens.org

### Preferences and Ratings

* Preference Model
  * Explicit: Rating, Review, Vote
  * Implicit: Click, Purchase, Follow

#### Explicit Ratings

* Just ask the users what they think!
  * e.g. Star Ratings, Thumbs and Likes

* When are ratings provided?
  * Consumption — during or immediately after experiencing the item
  * Memory — some time after experience
  * Expectation — the item has not been experienced

* Difficulties with Ratings
  * Are ratings reliable and accurate?
  * Do user preferences change?
  * What does a rating mean?

#### Implicit Data

* Data collected from user actions
* Key difference: user action is for some other purpose, not expressing preference
* Their actions say a lot!
  * e.g. Reading Time, Binary actions (Click on link, Don't click on link, Purchase. Follow/Friend)

* Subtleties and Difficulties
  * What does the action mean?
    * Purchase: they might still hate it
    * Don't click: expect bad, or didn't see
  * How to scale/represent actions?
  * Lots of opportunity to be creepy
    * Education may help
    * So can respecting privacy

#### Conclusion

* Recommenders mine what users say and what they do to learn preferences
* Ratings provide explicit expressions of preference
* Implicit data benefits from greater volume

### Predictions and Recommendations

#### Predictions

* Estimates of how much you’ll like an item
  * Often scaled to match some rating scale
  * Often tied to search or browsing for specific products

#### Recommendations

* Recommendations are suggestions for items you might like (or might fit what you’re doing)
  * Often presented in the form of “top-n lists”
  * Also sometimes just placed in front of you

#### Prediction and Recommendation
* Often, the two come together
* Predictions:
  * Pro: helps quantify item
  * Con: provides something falsifiable
* Recommendations
  * Pro: provides good choices as a default
  * Con: if perceived as top-n, can result in failure to explore (if top few seem poor)

#### Another dimension to consider
* How explicit is the prediction or recommendation (vs. organic)?
  * Historical note: we paid for it, we’ll let you know
  * Today: balance between explicit prediction (falsifiable) and coarser granularity (you might like this!)
  * Today: balance between theses are the best (top-n) and softer presentation (here are some that might be interesting)

### Taxonomy of Recommenders

#### Purposes of Recommendation

* The recommendations themselves
  * Sales
  * Information
* Education of user/customer
* Build a community of users/customers around products or content

#### Personalization Level

* Generic / Non-Personalized
  * Everyone receives same recommendations
* Demographic
  * Matches a target group
* Ephemeral
  * Matches current activity
* Persistent
  * Matches long-term interests

#### Interfaces

* Types of Output
  * Predictions
  * Recommendations
  * Filtering
  * Organic vs. explicit presentation
* Agent/Discussion Interface
* Types of Input
  * Explicit
  * Implicit

#### Recommendation Algorithms

* Non-Personalized Summary Statistics
* Content-Based Filtering
  * Information Filtering
  * Knowledge-Based
* Collaborative Filtering
  * User-User
  * Item-Item
  * Dimensionality Reduction
* Others
  * Critique / Interview Based Recommendations
  * Hybrid Techniques

#### Basic Model

* Users: User Attributes (demographics)
* Items: Item Attributes (properties, genres, etc)
* Ratings
* (Community)

#### Non-Personalized Summary Stats

* External Community Data
  * Best-seller; Most popular; Trending Hot
* Summary of Community Ratings
  * Best-liked
* Examples
  * Zagat restaurant ratings
  * Billboard music rankings
  * TripAdvisor hotel ratings

#### Content-Based Filtering

* User Ratings x Item Attributes => Model
* Model applied to new items via attributes
* Alternative: knowledge-based
  * Item attributes form model of item space
* Users navigate/browse that space
* Examples
  * Personalized news feeds
  * Artist or Genre music feeds

#### Personalized Collaborative Filtering

* Use opinions of others to predict/recommend
* User model – set of ratings
* Item model – set of ratings
* Common core: sparse matrix of ratings
  * Fill in missing values (predict)
  * Select promising cells (recommend)
* Several different techniques

#### Collaborative Filtering Techniques

* User-user
  * Select neighborhood of similar-taste people
  * Variant: select people you know/trust
  * Use their opinions
* Item-item
  * Pre-compute similarity among items via ratings
  * Use own ratings to triangulate for recommendations
* Dimensionality reduction
  * Intuition: taste yields a lower-dimensionality matrix
  * Compress and use a taste representation

#### Note on Evaluation

* To properly understand relative merits of each approach, we will spend significant time on evaluation
  * Accuracy of predictions
  * Usefulness of recommendations
    * Correctness
    * Non-obviousness
    * Diversity
  * Computational performance

#### Other Approaches

* Interactive recommenders
  * Critique-based, dialog-based
* Hybrids of various techniques
