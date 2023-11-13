---
layout: post
title: Review of Variational Auto-Encoders
subtitle: Variational Auto-Encoders
date: 2021-09-17 00:00:00 
---

This is a brief review of Variational Auto-Encoder (VAE) for my private research. 

# Two Main Roles of a VAE
  - Generative Modeling
  - Feature Representation 

# Model Structure 

One big picture of a VAE is predicated on a recursive structue in a sense that a model, normally neural network architecture, uses input data (called predictors or explanatory variables) as output data. This interesting structure leads to think about a hidden feature space (latent data) that is so sufficiently compressed inside the model as to contain all information of the input data. Specifically, the entire model can be identified with two phases: 

  - Encoder: a function from the input data to latent data
  - Decoder: a function from the latent data to the input data (practically output)

A primitive auto-encoder model might have just focused on *data reduction* to tackle problems mainly from high-dimensional inferences. It looks like that the field is called as the representation learning in the context of Computer Science. By the way, one remarkable extension in 2014 made a lot of researchers fascinated to delve into the model and its variants. The suggestion was to impose stochastic structure on the latent space, so that the model can mimic the original input data. In other words, the model has the capability *to reproduce a data generating process*. Considering it is difficult to assign a probability distribution on unstructured data such as image, video, words, etc., we can guess that VAEs would be promising field for a while because, in a lot of research and projects, there has been a lot of demand for processing these data. 


# Two(?) Research Points

To esimate the two functions, encoder and decoder, there has been research a lot to efficiently maximize the lower bound of the observed log likelihood. The bound is normally referred to as the evidence lower bound (ELBO). To the best of knowledge, there are two big research topics currently: 

  1. The first is to approximate posterior distribution of an encoder because the posterior distribution of the encoder is intractable. Variaional approaches, to overcome this issue, have generally been the first method to be considered in that the approach makes the estimation procedure as feasible. From deterministic to sampling approaches, a lot of techniques have been suggested in accordance with the development of computation algorithms. 

  2. Posterior collapse phenomenon is the second issue. In the model learning procedure, VAEs tend to get stuck to minimizing Kullback-Leibler divergence between a prior and a posterior distribution rather than maximizing the expected log likelihood, which brings about the lack of fit problem. One suggested, but seems powerful method is to anneal the part of the KL divergence term. 

A VAE also has a rival, a generative adversarial network (GAN), as other statistical methods. Although both of them aim to generate new samples based on the observed samples, they are fundamentally different in view of the target functions to be minimized or maxmimized. This is one of reasons why they each have their own advantages and disadvantages. 