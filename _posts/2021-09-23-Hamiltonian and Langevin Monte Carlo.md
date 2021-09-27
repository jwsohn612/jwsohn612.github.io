---
layout: post
title: Hamiltonian and Langevin Monte Carlo
subtitle: HMC
date: 2021-09-23 00:00:00 
use_math: true
---

The purpose of this post is to provide a big picture about how Hamiltonian Monte carlo works, instead of rigorous proof of the sampler. I am a statistician with elementary understanding of Physics. Be cautious my post might contain wrong explanation.

# Hamiltonian Dynamics 

To the best of my knowldege, Hamiltonian dynamics is a physical system where potential energy, $U(q)$, and kinetic energy, $K(p)$, preserves the total amount of energy, $H(q,p) = U(q) + K(p)$. $H$ implies Hamiltonian dynamics. For instance, if kinetic energy increases, potential energy decreases in accordance with the amount of the increment. This relationship  between two energy functions can be mathematically described through the partial differential equations (PDE). 

$\dfrac{d q_i}{dt} = \dfrac{dH}{dp_i}$

$\dfrac{d p_i}{dt} = -\dfrac{dH}{dq_i}$ 

for $i=1,\dots,d$, where $q$ and $p$ are d-dimensional vectors that each component is an element of potential and kinetic energy respectively.

# Hamiltonian Monte Carlo 

Hamiltonian Monte Carlo (HMC), as this name implies, harnesses characteristics of Hamiltonian dynamics. I don't want to discuss those properties here but just go through the algorithmic aspect of HMC conceptually. For this simple and basic explanation, coping with a canonical distribution would be sufficient. A canonical distribution of Hamiltonian dynamics is 

$H(p,q) = \dfrac{1}{Z}\exp(-\dfrac{1}{T}(U(q)+K(p)))$

where $T$ is temperature and $K(P)$ conventionally has the form of $p^{T}Mp/2$ which looks a normal distribution exactly. Remark that $U(q)$ plays a role of target distribution that we want to draw enough random samples for inferences. For example, we can specifiy $U(q) = -\log(p(q)p(D|q))$ for collecting samples from posterior distributions through HMC. 

Similar to other Monte Carlo based algorithms, HMC also consists of two phases: proposing a new candidate and evaluating the proposed one with an acceptance probability. More specifically, in HMC, solving Hamiltonian dynamics becomes the proposal step, and after that, the evaluation process is followed. What does the solving equation mean?

## Approximated Hamiltonian Dynamics

As the PDEs implies, each element in both energy functions has the derivative with respect to time $t$, and its amount of change is equivalent to the derivative of Hamiltonian dynamics with respect to a corresponding element of other energy function. Unfortunately, getting analytical solution of PDE problems extremely difficult in general, so we depend on some approximation methods, some of which dicretizes a continuous domain. In HMC, trajectories of each energy function along the time which is continous are analysis interests.  

### Euler's method

One basic approach called Euler's method is based on approximating derivatives of $q(t)$ and $p(t)$ by the amount of small size $\epsilon$. Let $K(p)=\sum_{i=1}^{d} p_i^2/m_i$. Then, the numerical derivatives are
1. $\dfrac{dp_i(t)}{dt}\approx \dfrac{p_i(t+\epsilon)-p_i(t)}{\epsilon}$
2. $\dfrac{dq_i(t)}{dt}\approx \dfrac{q_i(t+\epsilon)-q_i(t)}{\epsilon}$,

and this approximation can also be expressed as 
1. $p_i(t+\epsilon) \approx p_i(t) + \epsilon \frac{dp_i(t)}{dt}$ = $p_i(t+\epsilon) \approx p_i(t) - \epsilon \frac{dU(q)}{dq_i}$
2. $q_i(t+\epsilon) \approx q_i(t) + \epsilon \frac{dq_i(t)}{dt}$ = $q_i(t+\epsilon) \approx q_i(t) + \epsilon \frac{p_i(t)}{m_i}$.

Therefore, we can obtain values of $p(t)$ and $q(t)$ at $t=\tau$ from arbitrary initial values $p(0)$ and $q(0)$. For getting the RHS, you need to plug the PDE equations into these derivatives equations. 


### Leapfrog method

Euler's method, however, has poor approximation so practically not useful, although it has simple implementation. Leapfrog method is a tricky extension of Euler's method but has good approximation. This method takes more care of approximating the kinetic variable $p$. Now $p(t)$ is estimated two times within $\epsilon$ movement while $q(t)$ justs moves one time in $\epsilon$. To be specific, 
1. $p_i(t+\epsilon/2) \approx p_i(t) - \epsilon/2 \frac{dU(q)}{dq_i}$
2. $q_i(t+\epsilon) \approx q_i(t) + \epsilon \frac{p_i(t+\epsilon/2)}{m_i}$.
3. $p_i(t+\epsilon) \approx p_i(t+\epsilon) - \epsilon/2 \frac{dU(q(t+\epsilon))}{dq_i}$

## HMC algorithm

So far, I have described the proposal step of HMC which is actually equivalent to the approximation of PDEs. Algorithm flow of HMC might help you figure out HMC more clearly. To simplify description, assume $d=1$. For each iteration, 

1. Generate $p_0 \sim {\rm N}(0, m)$
2. Set $(q',p') = (q(\tau), p(\tau))$ through Leapfrog method with the initial value $p_0$. Note $\tau$ and $\epsilon$ are hyperparameters.
3. Negation of p', p' = -p'
4. Evaluating $(q',p')$ with a probability $\min(1,H(q',p')/H(q,p)=\min(1, \exp(-U(q')+U(q)-K(p')+K(p)))$

What we need to closely look at is Step 2 and 3. To put it simply, Step 3 is introduced to make sure that the deterministic transition in Step 2 inherits all appropriate properties with which a transition kernel in the context of Markov Chain Monte Carlo (MCMC) is equip. 

# Langevin dynamics 

Langevin dynamics is a special case of Hamiltonian dynamics when the approximation is executed just one time. Hence, performance is not as good as HMC in general. Interestingly however, it has been discoverd when $d$ is high-dimensional, Langevin Monte Carlo (LMC) works fine as well. Considering a lot of machine learning models require countless parameters nowadays, approximated Bayesian computing algorithm based on Langevin dynamics is able to be prioritized.


# Reference
 - Brooks, Steve, et al., eds. Handbook of markov chain monte carlo. CRC press, 2011.