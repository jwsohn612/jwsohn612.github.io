---
layout: post
title: Hamiltonian and Langevin Monte Carlo
subtitle: HMC
date: 2021-09-23 00:00:00 
use_math: true
---

# Hamiltonian Dynamics 

To the best of my knowldege, Hamiltonian dynamics is a physical system where potential energy and kinetic energy preserves the total amount of energy. For example, if kinetic energy increases, potential energy decreases in accordance with the amount of the increment. This relationship  between two energy functions can be mathematically described through the partial differential equations. All in all, we obtain 

$$H(q,p) = U(q) + K(p)$$

where U(q) and K(p) are respectively a potential and kinetic energy function with variable q and p. The function H implies Hamiltonian dynamics. 

# Hamiltonian Monte Carlo 

Hamiltonian Monte Carlo (HMC), as this name implies, harnesses characteristics of Hamiltonian dynamics. To figure out how HMC works, one important distribution property should be discussed. That is called a canonical distribution of Hamiltonican dynamics and the distribution said to be 

$$H(p,q) = \frac{1}{Z}\exp(-(U(q)+K(p))/T)$$

where $T$ is temperature and $K(P)$ conventionally has the form of $p^{T}Mp/2$ which looks a normal distribution exactly. 


# Reference
 - Neal