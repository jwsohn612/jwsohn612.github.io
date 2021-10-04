---
layout: post
title: Variable Selection via sparse prior
subtitle: Paper Review
date: 2021-09-23 00:00:00 
use_math: true
---

This is review of "Bayesian Linear Regression with Sparse Priors", Castillo, et al., 2018, The Annals of Statistics. This post is just written for individual study, so do not expect something special beyond the paper. 

# Sparse Linear Models 

Consider a model $Y=X\beta $ with $X \in \mathbb{R}^{n\times p}$ and $\beta \in \mathbb{R}^{p}$. Variable selection has been highly compelling when $n << p$ and many of componenets in $\beta$ are not significant, which is called a sparse linear model. Burgeoning of this field, diverse approaches have been suggested such as stepwise-selection, LASSO, etc.. 

What this paper especially works on is a 'sparse prior' whose structure consists of the following steps.

1. Select dimension of $\beta$: $s \sim \pi_{p}(s)$ on the set {$0,1,2,\dots,p\right$}

2. Select a subset $S \subset$ {$0,1,2,\dots,p$} whose cardinality is $|S|=s$

3. Assign a prior $g_S$ to $\beta_S := \{\beta_i; i\in S \}$ while $\beta_{S^c}$ has a measure only at zero. 

The authors discovered that this prior has some desirable properties in the context of variable selection with specific conditions of the sparse prior. 

# Prior Specification 

They assume that $g_S$ consists of $s$ independent Laplace distributions with a scale parameter $\lambda$: $g_S(\beta) \propto \lambda \exp(-\lambda|\beta|)$. The scale parameter has, however, is restricted on 

$dfrac{||X||}{p} \leq \lambda \leq 2\bar{\lambda},\quad\quad \bar{\lambda} = 2||X||\sqrt{\log p}.$

(I guess the purpose of this parameter constraint comes from mathematical convenience. The paper introduced some examples, but I could not figure out whether it has practical implication or not, to be honest.) 

One thing we have to care is that we should avoid the case that the shrinkage the penalty parameter brings about is mainly charge of the sparsity of $\beta$. In other words, the sparsity should result in the model selection via the sparse prior. For this reason, placing an appropriate dimension prior $\pi_p(s)$ is extremely critical. According to this demand, therefore, the authors impose an interesting structure:

$A_1 p^{-A_3}\pi_p(s-1) \leq \pi_p(s)\leg A_2 p^{-A_4} \pi_p(s-1),\quad s=0,1,\dots,p$ 

for some constants $A_1, A_2, A_3, A_4 >0$. It turns out that a 'Slap-and-Spike' prior is one example when the mixture weight comes from ${\rm Beta}(1,p^u), u>1$. 

# Requirements of Model Matrix



# Reference
 - Brooks, Steve, et al., eds. Handbook of markov chain monte carlo. CRC press, 2011.