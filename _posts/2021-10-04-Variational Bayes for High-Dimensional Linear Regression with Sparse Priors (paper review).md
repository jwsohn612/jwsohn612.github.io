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

In high-dimensional inference problems, selecting only necessary variables has been greatly in demand. Usually, so-called big data consists of numerous explanatory variables such as genomes, which leads to computational infeasibility of existing methods. To tackle this issue, the authors proposed using variational Bayes for the estimation of linear models with sparse priors. The benefits of harnessing the sparse priors are well demonstrated in the reference paper. Interestingly, the properties of the sparse priors can be observed even in case that a model adopts the variational Bayes for appoximating the posterior distribution with a great computational efficiency from ${\cal O}(2^p)$ to ${\cal O}(p)$ 



# Reference
 - Ray, Kolyan, and Botond Szabó. "Variational Bayes for high-dimensional linear regression with sparse priors." Journal of the American Statistical Association (2021): 1-12.
 - Castillo, Ismaël, Johannes Schmidt-Hieber, and Aad Van der Vaart. "Bayesian linear regression with sparse priors." The Annals of Statistics 43.5 (2015): 1986-2018.