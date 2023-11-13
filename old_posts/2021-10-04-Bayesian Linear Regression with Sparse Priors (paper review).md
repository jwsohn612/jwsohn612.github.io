---
layout: post
title: Bayesian Linear Regression with Sparse Priors (paper review)
subtitle: Paper Review
date: 2021-10-04 00:00:00 
use_math: true
---

This is review of "Bayesian linear regression with sparse priors.", Castillo, et al.,  The Annals of Statistics 43.5 (2015): 1986-2018. This post is just written for individual study, so do not expect something special beyond the paper. 

# Sparse Linear Models 

Consider a model $Y=X\beta $ with $X \in \mathbb{R}^{n\times p}$ and $\beta \in \mathbb{R}^{p}$. Variable selection has been highly compelling when $n << p$ and many of componenets in $\beta$ are not significant, which is called a sparse linear model. The burgeoning of this field, diverse approaches have been suggested such as stepwise-selection, LASSO, etc.. 

What this paper especially works on is a 'sparse prior' whose structure consists of the following steps.

1. Select dimension of $\beta$: $s \sim \pi_{p}(s)$ on the set {$0,1,2,\dots,p $}

2. Select a subset $S_\beta \subset${$0,1,2,\dots,p$} whose cardinality is |$S$|$=s$

3. Assign a prior $g_S$ to $\beta_S :=$ {$\beta_i; i\in S$} while $\beta_{S^c}$ has a measure only at zero. 

The authors discovered that this prior has some desirable properties in the context of variable selection with specific conditions of the sparse prior. 

# Prior Specification 

They assume that $g_S$ consists of $s$ independent Laplace distributions with a scale parameter $\lambda$. The scale parameter has, however, is bounded by the norm of model matrix and its dimension. 

One thing we have to care is that we should avoid the case that the shrinkage the penalty parameter brings about is mainly charge of the sparsity of $\beta$. In other words, the sparsity should result in the model selection via the sparse prior. For this reason, placing an appropriate dimension prior $\pi_p(s)$ is extremely critical. According to this demand, therefore, the authors impose an interesting structure:

$A_1 p^{-A_3}\pi_p(s-1) \leq \pi_p(s) \leq A_2 p^{-A_4} \pi_p(s-1),\quad s=0,1,\dots,p$ 

for some constants $A_1, A_2, A_3, A_4 >0$. It turns out that a 'Slap-and-Spike' prior is one example when the mixture weight comes from ${\rm Beta}(1,p^u), u>1$. 

# Requirements of estimability

In general, each coefficient of a high-dimensional linear model cannot be estimated due to linearly dependent columns. However, it is known that the local invertability of Gram matrix is sufficient condition for estimability of high-dimensional linear models if $\beta$ is sparse. This estimable condition, in this paper, is represented as the compatibility number $\phi(S)$. Refer to the paper if you want to check the detail of the number. In addition, they also introduced uniform compatibility, smallest scaled sparse singular value, and mutual coherence, which are used to prove desirable properties of the sparse prior. 

# Theoretical Properties of this prior

With the above assumptions and requirements, they stated that the prior provides:  
 
1. (Dimension): The dimension of $S_\beta$ does not overshoot over the true dimension by more than a factor in the sense of posterior distribution. 

2. (Recovery): The true data is recovred from the posterior distribution in terms of the predictive error and parameter estimations in the sense of $L_1$ and $L_2$ norms. 

3. (Oracle Recovery): The authors showed an oracle inequality with a posterior probability measure can be dissused with respect to $L_1$, $L_2$, and $L_\infty$. The second property actually can be induced from this oracle inequality. 

4. (No supersets): Coefficients of a model will be a subset of a true set of coefficients.

5. (Selection): Non-zero coefficients are included in $S_\beta$


# Approximation of posterior distributions 


When a model is predicated on the small lambda regime, the posterior distribution is approximated to a random mixture distribution between a normal distribution and the Dirac-delta measure at zero.  To be specific, assume the model is given a well selected set $S$ and the prior distribution satisfies the above conditions. Then, by Bernstein-Von Mises, the asymptotic posterior distribution of $S$ has the mixture distribution around the neighborhood of the true coefficients and it converges to the true posterior distribution (when data dominates the shrinkage induced by the prior parameters).


# Reference
 - Castillo, Ismaël, Johannes Schmidt-Hieber, and Aad Van der Vaart. "Bayesian linear regression with sparse priors." The Annals of Statistics 43.5 (2015): 1986-2018.