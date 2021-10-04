---
layout: post
title: Variable Selection via sparse prior
subtitle: HMC
date: 2021-09-23 00:00:00 
use_math: true
---

This is review of "Bayesian Linear Regression with Sparse Priors", Castillo, et al., 2018, The Annals of Statistics. This post is just written for individual study, so do not expect something special beyond the paper. 

# Sparse Linear Models 

Consider a model $Y=X\beta $ with $X \in \mathbb{R}^{n\times p}$ and $\beta \in \mathbb{R}^{p}$. Variable selection has been highly compelling when $n << p$ and many of componenets in $\beta$ are not significant, which is called a sparse linear model. Burgeoning of this field, diverse approaches have been suggested such as stepwise-selection, LASSO, etc.. 

What this paper especially works on is a 'sparse prior' whose structure consists of the following steps.

1. Select dimension of $\beta$: $s \sim \pi_{p}$ on the set $\left\{0,1,2,\dots,p\right\right\}$

2. Select a subset $S \subset \{0,1,2,\dots,p\} $ whose cardinality is $|S|=s$

3. Assign a prior $g_S$ to $\beta_S := \{\beta_i; i\in S \}$ while $\beta_{S^c}$ has a measure only at zero. 

The authors discovered that this prior has some desirable properties in the context of variable selection with specific conditions of the sparse prior. 

# Prior Specification 

They assume that $g_S$ consists of $s$ independent Laplace distributions with a scale parameter $\lambda$: $g_S(\beta) \propto \lambda \exp(-\lambda|\beta|)$. The scale parameter has, however, is restricted on 

$dfrac{||X||}{p} \leq \lambda \leq 2\bar{\lambda},\quad\quad \bar{\lambda} = 2||X||\sqrt{\log p}$



# Reference
 - Brooks, Steve, et al., eds. Handbook of markov chain monte carlo. CRC press, 2011.