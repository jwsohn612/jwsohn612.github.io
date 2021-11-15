---
layout: post
title: Variational Bayes for High-Dimensional Linear Regression with Sparse Priors (paper review)
subtitle: Paper Review
date: 2021-10-04 00:00:00 
use_math: true
---

This is review of "Variational Bayes for High-Dimensional Linear Regression with Sparse Priors", Ray and Szabó,  JASA (2020). This post is just written for individual study, so do not expect something special beyond the paper. 

The main reference paper of this research is "Bayesian linear regression with sparse priors.", Castillo, et al., The Annals of Statistics (2015). The authors used the same notations, and furthermore they discussed theoretical properties of the suggested method based on the main reference. 

# Motivation

In high-dimensional inference problems, selecting only necessary variables has been greatly in demand. Usually, so-called big data consists of numerous explanatory variables such as genomes, which leads to computational infeasibility of existing methods. To tackle this issue, the authors proposed using variational Bayes for the estimation of linear models with sparse priors. The benefits of harnessing the sparse priors are well demonstrated in the reference paper. Interestingly, the properties of the sparse priors can be observed as well even in case that a model adopts the variational Bayes for appoximating the posterior distribution with reducing model complexity from ${\cal O}(2^p)$ to ${\cal O}(p)$. 

# Main Idea 

The great computational benefit arises from the mean-field assumption in the field of variational inference. In statisical language, the assumption means that all variables are statistically independent. If variables construct complex hierarchy of a model, it is obvious that the model would have the lack of fit. However, cutting out dependency between variables might not lose much information in variable selection procedure intuitively as well as make the approximation procedure scalable. For instance, if we have $p$ variables and assign $r_i,\, i=1,2,\dots,p$ indicator variables to select the best model, the model amounts to calculating all possible $2^p$ cases. In case that, however, we use the mean-field assumption, the given model is not interested in association among $r_i$ anymore but only in $p$ cases. This is the fundamental idea of this research although the model is not capable of coping with pairwise information.

# Algorithm Issue 

Although this suggested idea brings about great coomputation efficiency, the method is very sensitive with respect to the initial values of Coordinate Ascent Variational Inference (CAVI) algorithm. They addressed nonconvex optimization structure of variational Bayes results in such an issue and proposed the prioritized update rule of CAVI with empirical studies. The prioritized CAVI updates the non-zero coefficients first from the biggest one. By doing so, the algorithm to the simplified probability space reaches out much better approximation.

# Takeaway 

- This research shows the brutal approximation still satisfyies some properties of sparse priors with respect to basic linear regression models. 
- Although the concept seems to imply the approximation technique brings great computational efficiency, the simulation study demonstrates the proposed method does not defeat some competiting methodologies in fact.
- Is it possible to assign latent variables for the pairwise information so that the model is enough to explain multicolinearity which is one of irritating obstacles against successful variable selection in practice?

# Reference
 - Ray, Kolyan, and Botond Szabó. "Variational Bayes for high-dimensional linear regression with sparse priors." Journal of the American Statistical Association (2021): 1-12.
 - Castillo, Ismaël, Johannes Schmidt-Hieber, and Aad Van der Vaart. "Bayesian linear regression with sparse priors." The Annals of Statistics 43.5 (2015): 1986-2018.