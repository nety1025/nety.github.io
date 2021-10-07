---
title:  "Notes on Bandit Model"
mathjax: true
layout: post
categories: modeling
comments: true
---

## Bayesian Updating (Learning) about the Mean of a Normally Distributed Variable

Assuming there are n periods. In each period t, the payoff is $$X_i=u+e_{i,t}$$ where $$e_{i,t} /sim N(0, /sigma_e)$$. The mean is drawn at the start of period 1 from a normal distribution $$u /sim N(0, /sigma_u)$$, and it remains the same in the following periods. We don't know the mean, and will have to learn it through observing the signal $$X_is$$.

So, after observing n signals $x_1, x_2, ..., x_n$, the best estimate of $u$ is:
$$ E[u|x_1, x_2, ..., x_n]=\frac{\sigma_u^2 \bar{x}}{\sigma_u^2+{\frac {\sigma_e^2}N}} $$

The above expected value can be derived from the formula on the note [Bayesian Inference for the Normal Distribution](http://www.ams.sunysb.edu/~zhu/ams570/Bayesian_Normal.pdf). The posterior distribution of $u$ with a sample size of n has the following property:
$$ \sigma_1^2=(\frac {1}{\sigma_u^2} + \frac {1}{\frac {\sigma_e^2}n})^{-1} $$
$$ \mu_1=\sigma_1^2(\frac{\mu_u}{\sigma_u^2} +\frac {\bar{x}}{\frac {\sigma_e^2}{n}}) $$

where $\mu_u=0$, and $\bar{x}$ is the average value of the n signals observed. Also note that we have ${\sigma_e^2}$ in the formula because $E[x_i|u] \sim N(u, \sigma_e)$.

## Reference
* [31 - Normal prior conjugate to normal likelihood - proof 1](https://www.youtube.com/watch?v=MUhsT0U_nxY)
* [32 - Normal prior conjugate to normal likelihood - proof 2](https://www.youtube.com/watch?v=OGxHNPYLtko)
* [33 - Normal prior conjugate to normal likelihood - intuition](https://www.youtube.com/watch?v=f3o9Crx3qx4)
* [Bayesian Inference for the Normal Distribution](http://www.ams.sunysb.edu/~zhu/ams570/Bayesian_Normal.pdf)
