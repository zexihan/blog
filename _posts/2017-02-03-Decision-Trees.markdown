---
layout:     post
title:      "Decision Trees"
subtitle:   "Machine Learning"
date:       2017-02-03
author:     "Zexi"
header-img: "img/post-bg-unix-linux.jpg"
catalog: true
tags:
    - Machine Learning
---

# Decision Trees

**Decision Trees (DTs)** are a non-parametric supervised learning method used for [classification](http://scikit-learn.org/stable/modules/tree.html#tree-classification) and [regression](http://scikit-learn.org/stable/modules/tree.html#tree-regression). The goal is to create a model that predicts the value of a target variable by learning simple decision rules inferred from the data features.

Some advantages of decision trees are:

> - Simple to understand and to interpret. Trees can be visualised.
> - Requires little data preparation. Other techniques often require data normalisation, dummy variables need to be created and blank values to be removed. Note however that this module does not support missing values.
> - The cost of using the tree (i.e., predicting data) is logarithmic in the number of data points used to train the tree.
> - Able to handle both numerical and categorical data. Other techniques are usually specialised in analysing datasets that have only one type of variable. See [algorithms](http://scikit-learn.org/stable/modules/tree.html#tree-algorithms) for more information.
> - Able to handle multi-output problems.
> - Uses a white box model. If a given situation is observable in a model, the explanation for the condition is easily explained by boolean logic. By contrast, in a black box model (e.g., in an artificial neural network), results may be more difficult to interpret.
> - Possible to validate a model using statistical tests. That makes it possible to account for the reliability of the model.
> - Performs well even if its assumptions are somewhat violated by the true model from which the data were generated.

The disadvantages of decision trees include:

> - Decision-tree learners can create over-complex trees that do not generalise the data well. This is called overfitting. Mechanisms such as pruning (not currently supported), setting the minimum number of samples required at a leaf node or setting the maximum depth of the tree are necessary to avoid this problem.
> - Decision trees can be unstable because small variations in the data might result in a completely different tree being generated. This problem is mitigated by using decision trees within an ensemble.
> - The problem of learning an optimal decision tree is known to be NP-complete under several aspects of optimality and even for simple concepts. Consequently, practical decision-tree learning algorithms are based on heuristic algorithms such as the greedy algorithm where locally optimal decisions are made at each node. Such algorithms cannot guarantee to return the globally optimal decision tree. This can be mitigated by training multiple trees in an ensemble learner, where the features and samples are randomly sampled with replacement.
> - There are concepts that are hard to learn because decision trees do not express them easily, such as XOR, parity or multiplexer problems.
> - Decision tree learners create biased trees if some classes dominate. It is therefore recommended to balance the dataset prior to fitting with the decision tree.

## Mathematical formulation

Given training vectors $xi\in R^n, i=1,…, l$ and a label vector $y\in R^l$, a decision tree recursively partitions the space such that the samples with the same labels are grouped together.

Let the data at node m be represented by $Q$. For each candidate split $\theta=(j,t_m)$ consisting of a feature $j$ and threshold $t_m$, partition the data into $Q_{left}(\theta)$ and $Q_{right}(\theta)$ subsets

$$Q_{left}(\theta)=(x,y)|x_j<=t_m$$

$$Q_{right}(\theta)=Q ∖ Q_{left}(\theta)$$

The impurity at m is computed using an impurity function $H()$, the choice of which depends on the task being solved (classification or regression)

$$G(Q,\theta)=\frac{n_{left}}{N_m}H(Q_{left}(\theta))+\frac{n_{right}}{N_m}H(Q_{right}(\theta))$$

Select the parameters that minimises the impurity

$$\theta^∗=\text{argmin}_\theta ⁡G(Q,\theta)$$

Recurse for subsets $Q_{left}(\theta^∗)$ and $Q_{right}(\theta^∗)$ until the maximum allowable depth is reached, $N_m<\text{min}_{samples}$ or $N_m=1$.

### Classification criteria

If a target is a classification outcome taking on values $0,1,…,K-1​$, for node $m​$, representing a region $R_m​$ with $N_m​$ observations, let

$$p_{mk}=1/N_m\sum_{x_i\in R_m}I(y_i=k)$$

be the proportion of class k observations in node $m$

Common measures of impurity are Gini

$$H(X_m)=\sum_k p_{mk}(1−p_{mk})$$

Cross-Entropy

$$H(X_m)=−\sum_k p_{mk}log⁡(p_{mk})$$

and Misclassification

$$H(X_m)=1−\text{max}(p_{mk})$$

where $X_m$ is the training data in node $m$

### Regression criteria

  If the target is a continuous value, then for node $m$, representing a region $R_m$ with $N_m $observations, common criteria to minimise as for determining locations for future splits are Mean Squared Error, which minimizes the L2 error using mean values at terminal nodes, and Mean Absolute Error, which minimizes the L1 error using median values at terminal nodes.

Mean Squared Error:

$$\bar{y}_m=\frac{1}{N_m}\sum_{i\in N_m}y_i$$

$$H(X_m)=\frac{1}{N_m}\sum_{i\in N_m}(y_i−\bar{y}_m)^2$$

Mean Absolute Error:

$$\bar{y}_m=\frac{1}{N_m}\sum_{i\in N_m}y_i$$

$$H(X_m)=\frac{1}{N_m}\sum_{i\in N_m}|y_i−\bar{y}_m|$$

where $X_m$ is the training data in node $m$


 **Reference**

http://scikit-learn.org/stable/modules/tree.html

