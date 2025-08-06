# Bayesian Monte Carlo Markov Chain modelling Cookbook

## Motivation

While there are many dedicated packages to perform Bayesian estimation, it might be very useful to understand what's under the hood, so scientists and students are increasingly knowledgeable and empowered to make the best modelling decisions. This cookbook is an effort to demonstrate how Monte Carlo Markov Chain (MCMC) algorithms play a role in Bayesian estimation. I have included several applications of this algorithm to different models - starting from estimating basic normal distribution parameters and regressions, all the way to more complex population dynamics models.

## Authors

[Matheus de Barros](https://github.com/matheusbarrosb)

### Background

#### Bayes' rule and probability statements

### Bayes' Rule and Probability Statements

In Bayesian models, the goal is to estimate the posterior distribution given a prior distribution p(θ) (what we already know about the parameters), and the likelihood p(y|θ) (the probability of observing the data y given parameter values θ).

To estimate model parameters θ given data y, we specify the joint probability distribution:

p(y, θ) = p(y|θ) p(θ)

Applying Bayes' rule:

p(θ|y) = [p(y|θ) p(θ)] / p(y)

where p(y) = ∫ p(y|θ) p(θ) dθ is the marginal likelihood (normalizing constant).

The unnormalized posterior can be written as:

p(θ|y) ∝ p(θ) p(y|θ)

---

#### The Metropolis-Hastings Algorithm

**Steps:**

1. **Initialization:** Start with an initial value θ(0).
2. **Proposal:** At iteration t, propose θ* from q(θ* | θ(t-1)).
3. **Acceptance Ratio:** Compute

   α = min(1, [p(θ*) q(θ(t-1)|θ*)] / [p(θ(t-1)) q(θ*|θ(t-1))])

4. **Accept or Reject:** Draw u ~ Uniform(0,1).
   - If u < α, set θ(t) = θ*.
   - Else, θ(t) = θ(t-1).
5. **Iterate:** Repeat steps 2–4 as needed.

**Key Properties:**
- The sequence {θ(t)} forms a Markov chain with stationary distribution p(θ).
- Only the unnormalized target distribution is needed.
- After burn-in, use remaining samples for posterior inference.

#### Key Properties

- The sequence of theta^(t) forms a Markov chain whose stationary distribution is the target distribution p(theta).
- The algorithm only requires knowing the target distribution up to a normalizing constant.
- After discarding an initial "burn-in" period, the remaining samples can be used to make inferences about the posterior distributions of the parameters.
