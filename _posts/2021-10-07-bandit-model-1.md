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

where $$\mu_u=0$$, $$\bar{x}$$ is the average value of the n signals observed, and we have $${\sigma_e^2}$$ in the formula because $$ E[X_i||u] \sim N(u, \sigma_e) $$.

So, after observing n signals $$x_1, x_2, ..., x_n$$, the best estimate of $$u$$ is:

$$ E[u|x_1, x_2, ..., x_n]=\frac{\sigma_u^2 \bar{x}}{\sigma_u^2+{\frac {\sigma_e^2}n}} $$

## Conditional Expectation
For any normally distributed random variable $$z$$, 

$$ E[z||z>0]=u_z+\sigma_z \frac {\phi(a)}{1-\Phi(a)} $$, 

where $$\phi()$$ is the probability density function of a standard normal distribution $$N(0,1)$$, and $$a=-\frac {u_z}{\sigma_z}$$.

Proof:

$$ E[z|z>0]= \frac {\int_{0}^{+\infty} zf_z(z)dz}{p(z>0)} $$

$$ = \frac {\int_{-\infty}^{+\infty} zf_z(z)dz - \int_{-\infty}^{0} zf_z(z)dz}{p(z>0)} $$

First, $$ \int_{-\infty}^{+\infty} zf_z(z)dz = u_z $$

Second, let $$ t= \frac {z-u_z}{\sigma_z} $$, $$ p(z>0) = p(t>\frac {z-u_z}{\sigma_z}) = p(t>a) = 1-\Phi(a) $$

Finally, $$ \int_{-\infty}^{0} zf_z(z)dz = \int_{-\infty}^{a} (t\sigma_z+u_z)f_t(t)dt $$
$$ = \int_{-\infty}^{a} t\sigma_zf_t(t)dt + \int_{-\infty}^{a} u_zf_t(t)dt $$
$$ = -\sigma_z \int_{-\infty}^{a} f'_t(t)dt + u_z \int_{-\infty}^{a} f_t(t)dt $$
$$ = -sigma_z\phi(a)+u_z\Phi(a)

Here, I used a special property of standard normal distribution: $$ tf_t(t)=-f'(t) $$

Pugging in the above terms, we can get $$ E[z||z>0]=u_z+\sigma_z \frac {\phi(a)}{1-\Phi(a)} $$.

I was struggling because at first, I did not realize $$ \int_{0}^{+\infty} zf_z(z)dz = E[z|z>0]p(z>0) $$. 

## Bayesian Updating (Learning) about A Success Probability

Given a Bernoulli random variable that has payoff +1 with probability q and payoff 0 with probability 1-q. We do not know the value of q, only that it is drawn from a uniform distribution $$ U(0,1) $$. Suppose we conduct N trials to find out more about q, and we observe S successes.

Then, we will have 

$$ E[q|S, N] = \frac {S+1}{N+2} $$

The proof can take reference from [here](https://ocw.mit.edu/courses/mathematics/18-443-statistics-for-applications-spring-2015/lecture-notes/MIT18_443S15_LEC8.pdf), using the definition of expectation and beta distribution. However, I learned an intersting intuition from my friend: Before any experiment, the expected value of q is $$1/2$$. After N trials, we add the total number of trials N to the denominator, and the number of successes S to the numerator. If we hold strong belief of the prior belief, we can enlarge $$1/2$$ to, say $$10,000/20,000$$, then the result of the experiments will affect the belief less.

A reminder to myself: **Intuition is very important!!**


## Reference
* [Bandit Models](https://sites.google.com/view/tomsociety/2021-summer-school/156-bandit-models?authuser=0)
* [Bayesian Inference for the Normal Distribution](http://www.ams.sunysb.edu/~zhu/ams570/Bayesian_Normal.pdf)
* [31 - Normal prior conjugate to normal likelihood - proof 1](https://www.youtube.com/watch?v=MUhsT0U_nxY)
* [32 - Normal prior conjugate to normal likelihood - proof 2](https://www.youtube.com/watch?v=OGxHNPYLtko)
* [33 - Normal prior conjugate to normal likelihood - intuition](https://www.youtube.com/watch?v=f3o9Crx3qx4)
* [Parameter Estimation, Fitting Probability Distributions, Bayesian Approach](https://ocw.mit.edu/courses/mathematics/18-443-statistics-for-applications-spring-2015/lecture-notes/MIT18_443S15_LEC8.pdf)