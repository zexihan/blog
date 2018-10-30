---
layout:     post
title:      "Ensemble Methods"
subtitle:   "Machine Learning"
date:       2017-06-15
author:     "Zexi"
header-img: "img/post-bg-unix-linux.jpg"
catalog: true
tags:
    - Machine Learning
---

# Bagging, Boosting, Stacking

All three are so-called "meta-algorithms": approaches to combine several machine learning techniques into one predictive model in order to decrease the variance (*bagging*), bias (*boosting*) or improving the predictive force (*stacking* alias *ensemble*).

Every algorithm consists of two steps:

1. Producing a distribution of simple ML models on subsets of the original data.
2. Combining the distribution into one "aggregated" model.

Here is a short description of all three methods:

1. **Bagging** (stands for **B**ootstrap **Agg**regat**ing**) is a way to decrease the variance of your prediction by generating additional data for training from your original dataset using [combinations with repetitions](http://en.wikipedia.org/wiki/Combinations) to produce [multisets](http://en.wikipedia.org/wiki/Multiset) of the same cardinality/size as your original data. By increasing the size of your training set you can't improve the model predictive force, but just decrease the variance, narrowly tuning the prediction to expected outcome.
2. **Boosting** is a two-step approach, where one first uses subsets of the original data to produce a series of averagely performing models and then "boosts" their performance by combining them together using a particular cost function (=majority vote). Unlike bagging, in the [classical boosting](http://www.cs.princeton.edu/courses/archive/spr08/cos424/readings/Schapire2003.pdf) the subset creation is not random and depends upon the performance of the previous models: every new subsets contains the elements that were (likely to be) misclassified by previous models.
3. **Stacking** is a similar to boosting: you also apply several models to your original data. The difference here is, however, that you don't have just an empirical formula for your weight function, rather you introduce a meta-level and use another model/approach to estimate the input together with outputs of every model to estimate the weights or, in other words, to determine what models perform well and what badly given these input data.

[![Comparative table](https://i.stack.imgur.com/RFfqb.png)](https://i.stack.imgur.com/RFfqb.png)

As you see, these all are different approaches to combine several models into a better one, and there is no single winner here: everything depends upon your domain and what you're going to do. You can still treat *stacking* as a sort of more advances *boosting*, however, the difficulty of finding a good approach for your meta-level makes it difficult to apply this approach in practice.

Short examples of each:

1. *Bagging*: [Ozone data](http://en.wikipedia.org/wiki/Bootstrap_aggregating).
2. *Boosting*: is used to improve [optical character recognition](http://en.wikipedia.org/wiki/Optical_character_recognition) (OCR) accuracy.
3. *Stacking*: is used in [classification of cancer microarrays](http://link.springer.com/article/10.1007/s13721-013-0034-x) in medicine.



**Reference**

https://stats.stackexchange.com/questions/18891/bagging-boosting-and-stacking-in-machine-learning