---
title:  "Notes on Bandit Model"
mathjax: true
layout: post
categories: modeling
comments: true
---

A bandit problem is a choice problem among uncertain alternatives while trying to maximize total payoff. It differs from searching problem in that in a bandit problem, the actor cares about payoff in each period. The key is to balance exploitation (go for the option with highest mean) and exploration (go for other options to learn more about them).

This note contains the points that I was struggling with when I learn the bandit model on [TOM Society](https://sites.google.com/view/tomsociety/2021-summer-school?authuser=0).


## Bayesian Updating (Learning) about the Mean of a Normally Distributed Variable

Assuming there are n periods. In each period t, the payoff is $$X_i=u+e_{i,t}$$ where $$e_{i,t} \sim N(0, \sigma_e)$$. The mean is drawn at the start of period 1 from a normal distribution $$u \sim N(0, \sigma_u)$$, and it remains the same in the following periods. We don't know the mean, and will have to learn it through observing the signal $$X_i$$s.

Based on the note [Bayesian Inference for the Normal Distribution](http://www.ams.sunysb.edu/~zhu/ams570/Bayesian_Normal.pdf), the posterior distribution of $$u$$ with a sample size of n is a normal distribution with the following parameters:

$$ \sigma^2=(\frac {1}{\sigma_u^2} + \frac {1}{\frac {\sigma_e^2}n})^{-1} $$

$$ \mu=\sigma^2(\frac{\mu_u}{\sigma_u^2} +\frac {\bar{x}}{\frac {\sigma_e^2}{n}}) $$

where $$\mu_u=0$$, $$\bar{x}$$ is the average value of the n signals observed, and we have $${\sigma_e^2}$$ in the formula because $$ E[X_i | u] \sim N(u, \sigma_e) $$.

So, after observing n signals $$x_1, x_2, ..., x_n$$, the best estimate of $$u$$ is:

$$ E[u|x_1, x_2, ..., x_n]=\frac{\sigma_u^2 \bar{x}}{\sigma_u^2+{\frac {\sigma_e^2}n}} $$

## Conditional Expectation
For any normally distributed random variable $$z$$, 

$$ E[z | z>0]=u_z+\sigma_z \frac {\phi(a)}{1-\Phi(a)} $$, 

where $$\phi()$$ is the probability density function of a standard normal distribution $$N(0,1)$$, and $$a=-\frac {u_z}{\sigma_z}$$.

Proof:

$$ E[z|z>0]= \frac {\int_{0}^{+\infty} zf_z(z)dz}{p(z>0)} $$

$$ = \frac {\int_{z=-\infty}^{+\infty} zf_z(z)dz - \int_{z=-\infty}^{0} zf_z(z)dz}{p(z>0)}

First, $$ \int_{z=-\infty}^{+\infty} zf_z(z)dz = u_z $$

Second, let $$ t= \frac {z-u_z}{\sigma_z} $$, $$ p(z>0) = p(t>\frac {z-u_z}{\sigma_z}) = p(t>a) = 1-\Phi(a) $$

We can also derive 

## Reference
* [31 - Normal prior conjugate to normal likelihood - proof 1](https://www.youtube.com/watch?v=MUhsT0U_nxY)
* [32 - Normal prior conjugate to normal likelihood - proof 2](https://www.youtube.com/watch?v=OGxHNPYLtko)
* [33 - Normal prior conjugate to normal likelihood - intuition](https://www.youtube.com/watch?v=f3o9Crx3qx4)
* [Bayesian Inference for the Normal Distribution](http://www.ams.sunysb.edu/~zhu/ams570/Bayesian_Normal.pdf)
* [Bandit Models](https://sites.google.com/view/tomsociety/2021-summer-school/156-bandit-models?authuser=0)
