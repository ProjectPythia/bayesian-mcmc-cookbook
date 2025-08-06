# Bayesian Monte Carlo Markov Chain modelling Cookbook

<img src="thumbnails/thumbnail.png" alt="thumbnail" width="300"/>

[![nightly-build](https://github.com/ProjectPythia/cookbook-template/actions/workflows/nightly-build.yaml/badge.svg)](https://github.com/ProjectPythia/cookbook-template/actions/workflows/nightly-build.yaml)
[![Binder](https://binder.projectpythia.org/badge_logo.svg)](https://binder.projectpythia.org/v2/gh/ProjectPythia/cookbook-template/main?labpath=notebooks)
[![DOI](https://zenodo.org/badge/475509405.svg)](https://zenodo.org/badge/latestdoi/475509405)

_See the [Cookbook Contributor's Guide](https://projectpythia.org/cookbook-guide) for step-by-step instructions on how to create your new Cookbook and get it hosted on the [Pythia Cookbook Gallery](https://cookbooks.projectpythia.org)!_

This Project Pythia Cookbook covers ... (replace `...` with the main subject of your cookbook ... e.g., _working with radar data in Python_)

## Motivation

While there are many dedicated packages to perform Bayesian estimation, it might be very useful to understand what's under the hood, so scientists and students are increasingly knowledgeable and empowered to make the best modelling decisions. This cookbook is an effort to demonstrate how Monte Carlo Markov Chain (MCMC) algorithms play a role in Bayesian estimation. I have included several applications of this algorithm to different models - starting from estimating basic normal distribution parameters and regressions, all the way to more complex population dynamics models.

## Authors

[Matheus de Barros](https://github.com/matheusbarrosb)

### Background

#### Bayes' rule and probability statements

In Bayesian models, the goal is to estimate the *posterior distribution* given a prior distribution \( p(\theta) \) (what we already know about the parameters), and the *likelihood* \( p(y\mid\theta) \) (the probability of observing the data \( y \) given parameter values \( \theta \)). Before introducing the MCMC algorithm, let's review some basics of probability and Bayes' rule.

To estimate model parameters \( \theta \) given the data \( y \), we need to specify the *joint probability distribution* of the data and parameters:

\( p(y, \theta) = p(y \mid \theta)\,p(\theta) \)

Applying Bayes' rule and conditional probability statements, we obtain the posterior:

\( p(\theta\mid y) = \frac{p(y \mid \theta)\,p(\theta)}{p(y)} \)

where \( p(y) = \int p(y \mid \theta)\,p(\theta)\,d\theta \) is the marginal likelihood, a normalizing constant. The marginal likelihood is the sum (or integral) over all possible values of \( \theta \) given the observed data.

The *unnormalized posterior* can be given by

\( p(\theta\mid y) \propto p(\theta)\,p(y\mid\theta) \)

---

#### The Metropolis-Hastings Monte Carlo Markov Chain algorithm

### The Metropolis-Hastings Algorithm

The **Metropolis-Hastings (MH) algorithm** is an MCMC method used to generate samples from a probability distribution when direct sampling is difficult due to not having an analytical solution. The Metropolis-Hastings algorithm is particularly useful for sampling from complex posteriors.

#### Steps of the Algorithm

1. **Initialization:**  
   Start with an initial value for the parameter(s) of interest, denoted as \( \theta^{(0)} \).

2. **Proposal:**  
   At iteration \( t \), propose a new value \( \theta^* \) from a proposal distribution \( q(\theta^* \mid \theta^{(t-1)}) \). This distribution is often chosen to be easy to sample from, such as a normal distribution centered at the current value (at iteration \( t-1 \)).

3. **Acceptance Ratio:**  
   This defines whether sampled values from the proposal distribution will be accepted or not.

   Compute the acceptance probability \( \alpha \):
   
   \( \alpha = \min\left(1, \frac{p(\theta^*)\,q(\theta^{(t-1)} \mid \theta^*)}{p(\theta^{(t-1)})\,q(\theta^* \mid \theta^{(t-1)})}\right) \)

4. **Accept or Reject:**  
   Draw \( u \sim \mathrm{Uniform}(0, 1) \).
   - If \( u < \alpha \), accept the proposal: set \( \theta^{(t)} = \theta^* \).
   - Otherwise, reject the proposal and set \( \theta^{(t)} = \theta^{(t-1)} \).

5. **Iterate:**  
   Repeat steps 2–4 for as many iterations as needed to construct a Markov chain of parameter samples.


#### Key Properties

- The sequence \( \{ \theta^{(t)} \} \) forms a Markov chain whose stationary distribution is the target distribution \( p(\theta) \).
- The algorithm only requires the target distribution to be known up to a normalizing constant.
- After discarding an initial "burn-in" period, the remaining samples can be used to make inferences on parameter posterior distributions.
