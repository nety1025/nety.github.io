---
title:  "Notes on Bandit Model"
mathjax: true
layout: post
categories: modeling
comments: true
---

## Bayesian Updating (Learning) about the Mean of a Normally Distributed Variable

Assuming there are N periods. In each period t, the payoff is $ N_i=u+e_{i,t} $ where $e_{i,t} /in N(0, /sigma_e)$. The mean is drawn at the start of period 1 from a normal distribution $u /in N(0, /sigma_u), and it remains the same in the following periods. We don't know the mean, and will have to learn it through observing the signal $X_is$.

So, after observing N signals $x_1, x_2, ..., x_N$, the best estimate of $u$ is:
$$ E[u|x_1, x_2, ..., x_N]=\frac {{\sigma_u^2}(x_1+x_2+...+x_N)}{{\sigma_u^2}+{\frac {\sigma_e^2}N}} $$

To arrive this formula, I relied on the formula related to the conjugate prior for the normal distribution.
