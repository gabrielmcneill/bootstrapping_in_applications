---
title: "Bootstrapping Workshop"
author: "Gabriel McNeill"
date: "2026-03-16"
output: 
  pdf_document:
    keep_md: true
---




# Preliminary Concepts

**Random variables**, \( X \), are functions defined on some sample space, \( \Omega \), that corresponds to the set of all (not necessarily observable) possible outcomes of a random experiment. Using mathematical notation, one can express this as: \( X: \Omega \rightarrow \mathcal{X} \). Random variables map particular outcomes in the sample space, $\omega \in \Omega$ (the domain), to a particular value among a set of values, \( \mathcal{X} \) (the range, or codomain). This set of values, \( \mathcal{X} \), could be the entire real line (i.e. \( \mathbb{R} \) or \( (-\infty, \infty) \) ) if we measure a continuous, unbounded quantity, it could be any subset of the real line, including positive integers \( \mathbb{Z} \), or any finite set (such as the numbers on a six sided die \( \{ 1, 2, 3, 4, 5, 6 \} \)). For each element in the range, \( x \in \mathcal{X} \), of a random variable, there is a subset, \( \Gamma \subset \Omega \) , of the sample space, \( \Omega \), with elements that each map to it: i.e. for all \( x \in \mathcal{X} \) there exists \( \Gamma \subset \Omega \) such that for every \( \omega \in \Gamma, X(\omega) = x \). Because \( X \) is a function, no element of the sample space maps to more than one value, \( x \in \mathcal{X} \). One way to think about a random variable is as a partition of a sample space into sets of equivalence classes. Whether or not \( X \) has an inverse, it always has a preimage \( X^{-1}(x) \), which is the set of all elements \( \omega \) in the sample space that map to the value \( x \): \( X^{-1}(x) = \{ \omega \in \Omega \mid X(\omega) = x\} \).

However, in applied statistics one typically forgets about the sample space altogether, and lets random variables simply stand for the observable outcomes that we measure in a real experiment. For example, if one rolls a die there is a large number of ways that a die can roll and tumble, possibly hitting other objects, and eventually come to rest (i.e. there are many events \( \omega \in \Omega \) that can map to a particular value of a random variable representing the die roll). But we really only care about particular observable outcomes, which in the case of the die roll is the value on the top side of the die. Although there could in principle be a very large sample space that represents all possible outcomes of rolling a die, we want to abstract that all away, and focus only on the six values. From this point on, we will speak no more of the sample space \( \Omega \), and only work with the range of a random variable - the set of values. 

Ideally, we would like to know the probability that a random experiment will produce a (random) variable of a particular value for each of the possible values the random variable can assume (i.e. for all \( x \in \mathcal{X} \) we want to know \( \mathbb{P}(X = x) \)). This is one way of saying that we would like to know the **distribution** of a random variable. If we could know the distribution of a random variable, we could make useful claims about the results of experiments that we analyze. For example, in the die rolling experiment, if we knew the probability of rolling each of the six possible values, we could make useful predictions about how often we'd expect to win in a game of craps. From the mathematical point of view, distributions are defined by probability measures, which are set functions mapping subsets of the range of a random variable to the closed interval [0, 1]. But we're not going to spend any time at all here on measure theory. It is sufficient to know that given a particular set of random variable outcomes \( \mathcal{X} \), a probability measure defined on that set (or more precisely on the actual random variable as a function) creates a distribution. 

Distributions are generally represented by distribution functions (continuous or discrete random variables), densities (continuous random variables), or probability mass functions (discrete random variables). Of these different representations, perhaps the most fundamental is the distribution function. 
Distributions can be indexed by values using a parametrization. When a parametrization is injective (one-to-one), the components of the parametrization are parameters. A **parameter**, \( \theta \), of a distribution with distribution function \( \mathrm{F} \), is a function of the the distribution: \( \theta = g(\mathrm{F}) \). One important way of defining parameters is through expectation, which is a common way of defining functions of probability distributions. For example, given a normally distributed random variable \( X \stackrel{iid}{\sim} \mathcal{N}(\mu, \sigma^{2}) \) with distribution function \( \mathrm{F}(x) = \int_{-\infty}^{x} d\mathrm{F}(z) = \int_{-\infty}^{x} \frac{1}{\sigma \sqrt{2 \pi}} e^{\frac{-1}{2} \left(\frac{z - \mu}{\sigma}\right)^{2}} dz \), we can express the mean parameter of the distribution as the expectation of the random variable:

$$ \mu = g(\mathrm{F}) = \mathbb{E}_{\mathrm{F}}[X] = \int_{-\infty}^{\infty} z d\mathrm{F}(z) = \int_{-\infty}^{\infty} \frac{z}{\sigma \sqrt{2 \pi}} e^{\frac{-1}{2} \left(\frac{z - \mu}{\sigma}\right)^{2}} dz $$

Similarly, we can represent the variance parameter of the distribution of \( X\) as

$$\sigma^{2} = f(\mathrm{F}) = \mathbb{E}_{\mathrm{F}}[(X - \mu)^{2}] = \int_{-\infty}^{\infty} (z - \mu)^{2} d\mathrm{F}(z) = \int_{-\infty}^{\infty} \frac{(z - \mu)^{2}}{\sigma \sqrt{2 \pi}} e^{\frac{-1}{2} \left(\frac{z - \mu}{\sigma}\right)^{2}} dz$$

A large class of parameters can be represented by viewing them as functions of a distribution defined through the expectation of a random variable (or of a function of a random variable). When parameters of distributions are defined through expectations of random variables distributed as the distribution of interest, one can estimate the distribution from a sample of data (e.g. empirical distribution) and then compute an estimate of the parameter as an expectation with respect to the estimated (or fitted) distribution. This is generally how the bootstrap works. A very quick preview: bootstrap estimators first find an approximation to the distribution of interest, and then determine parameter estimates by applying functions like \( g(\cdot) \) or \( f(\cdot) \) to the approximation of the distribution, and this happens easily when parameters are defined through expectation.

Parameters are often the characteristics of distributions that we want to know about. The mean, median, variance, standard deviation, correlation, skewness, kurtosis, and so on, are all parameters of some distribution of interest. Even the distribution function of a random variable \( X \) is a parameter that we may want to estimate.  

A more realistic model for the generation of data we actually see in the real world involves more than a single observation of the outcome of a random experiment. Typically, random experiments are performed many times, thereby generating a sequence of observations of random variables \( X_{1}, X_{2}, \dots , X_{n} \). Often, we can assume that each random variable in the sequence is independent of the others and has the exact same distribution as the others, i.e. \( X_{i} \stackrel{iid}{\sim} F \) for \( i \in \{1, 2, \dots, n\} \). Thus, it is almost always the case that the parameters of interest are defined with respect to the distribution of a function of the sequence of observed random variables. For example, given data \( \mathrm{X} = (X_{1}, X_{2}, \dots , X_{n}) \), we can let \( s(\mathrm{X}) = s(X_{1}, X_{2}, \dots , X_{n}) \) denote a function of the sequence of random variables to be observed (e.g. \( s(\mathrm{X}) = s(X_{1}, X_{2}, \dots , X_{n}) = \bar{X}_{n} = \frac{1}{n} \sum_{i = 1}^{n} X_{i} \)). Functions of the data defined in this way are called **statistical functionals** when the range of the function \( s(\cdot) \) is one-dimensional. For example, the sample mean, \( \bar{X}_{n} = s(X_{1}, X_{2}, \dots , X_{n}) \), is a statistical functional because despite having an n-dimensional input, the output is a single number (\( s: X_{1} \times X_{2} \times \cdots \times X_{n} \rightarrow \bar{X}_{n} \)). For \( s(X_{1}, X_{2}, \dots , X_{n}) \sim R \), the distribution of \( R \) is related to, but different from \( F \): it is the joint probability distribution of the independent and identically distributed \( X_{i} \). We learn in probability and statistics classes how to derive the distributions of functions of random variables whose distributions we know, and so we are used to parameters of \( R \) being determined or computed with respect to \( F \).

For example, when $s(\mathrm{X}) = \bar{\mathrm{X}} = \frac{1}{n} \sum_{i = 1}^{n} X_{i}$ (with $\bar{\mathrm{X}} \sim R$), we know that
$$\mathbb{E}_{R}[\bar{\mathrm{X}}] = \int \dots \int \frac{1}{n} (X_{1} + X_{2} + \dots + X_{n}) dF(x_{1}) \dots dF(x_{n}) = \frac{1}{n} \sum_{i = 1}^{n} \mathbb{E}_{F}[ X_{i}] = \frac{n \mu}{n} = \mu$$

Also, we know that
$$ \mathbb{V}ar_{R}[\bar{X}] = \mathbb{E}_{R}[(\bar{X} - \mu)^{2}] = \mathbb{E}_{R}[\bar{X}^{2}] - \mu^{2} = \int \dots \int \frac{1}{n^{2}} (X_{1} + X_{2} + \dots + X_{n})^{2} dF(x_{1}) \dots dF(x_{n}) - \mu^{2} = $$

$$ = \int \dots \int \frac{1}{n^{2}} \sum_{i = 1}^{n} X_{i}^{2} dF(x_{1}) \dots dF(x_{n}) - \mu^{2} = \frac{1}{n^{2}} \sum_{i = 1}^{n} \mathbb{E}_{F}[ X_{i}^{2}] - \mu^{2} = \frac{1}{n^{2}} n(\sigma^{2} + \mu^{2}) - \mu^{2} = \frac{\sigma^{2}}{n} $$
Note that because the data are iid, in these cases the expectation made with respect to the joint distribution of the statistical functional reduces to a function of expectations made with respect to the base (or marginal) distribution. This is the kind of helpful behavior that iid data tends to display. 

When analyzing data, \( X_{1}, X_{2}, \dots , X_{n} \), sampled iid from a distribution \( F \), we often perform analysis by observing the outputs of particular functions of these data. We don't just calculate means, medians, variances, correlation coefficients, and so on, we may also have reason to use a complicated algorithm that takes the data as input and outputs some number, \( s(X_{1}, X_{2}, \dots , X_{n}) \). In the case that \( s(\cdot) \) is the mean (\( s(\mathrm{X_{1}, X_{2}, \dots , X_{n}}) = \bar{\mathrm{X}}_{n} \)), we have memorized certain parameters of the distribution of \( s(\cdot) = \bar{\mathrm{X}}_{n} \). We know that the mean of the distribution of \( \bar{\mathrm{X}}_{n}\) is the mean of the distribution of each of the \( X_{i} \), as shown above. We also know that the variance of the distribution of \( \bar{\mathrm{X}}_{n} \) is \( \frac{\sigma^{2}}{n} \). However, in general cases, we may have no idea what the distribution of \( s(X) \) is, or how to analytically derive estimates of the parameters of that distribution that we might care about. This is where the usefulness of the bootstrap lies.

# Basic Bootstrap Theory

Before we can really understand what the bootstrap method is, and how it works, we need to introduce a few important concepts. And to set some basic assumptions. We assume in what follows that we have data \( x = (x_{1}, \dots, x_{n}) \) that is iid and is generated from the distribution \( F\). 

The first important concept is that of the **empirical distribution function** \( \hat{F} \). 

The empirical distribution function is completely determined by the data, \( x \). It is the distribution that puts probability of \( \frac{1}{n} \) at each observed datum, \( x_{i} \), for \( i = 1, \dots , n \). The empirical cumulative distribution function is defined as:

$$ \hat{F}_{n}(x) = \frac{1}{n} \sum_{i = 1}^{n} \mathbb{I}(x_{i} \leq x) $$
Where \( \mathbb{I}(x_{i} \leq x) \) is the indicator function that is equal to 1 when \( x \) is greater than or equal to \( x_{i} \), and 0 otherwise. Note that \( \hat{F}_{n}(x) \) is a step function (with range \( [0, 1] \)) that has jumps at each observed data value \( x_{i} \), the height of which is a multiple of \( \frac{1}{n} \).

The empirical distribution is a discrete distribution, and a non-parametric estimator for the true distribution \( F \). As an estimator, it has desirable properties, such as consistency, unbiasedness, and asymptotic normality. Because it is a functional estimator, it is a random function, or in other words, it is a function-valued random variable, which requires advanced mathematical theory for the derivation of its statistical properties (empirical process theory).

The empirical distribution function is fundamentally important in statistics. However, here we mainly need to be familiar with it in order to understand the next important concept: the plug-in principle. 

The second important concept, the **plug-in principle**, is a method for finding estimators, \( \hat{\theta} \), of parameters \( \theta = t(F) \) of a distribution \( F \). The plug-in principle is just a way of finding estimators of a particular parameter. Given a function \( t(\cdot) \) defined on a family of distributions \( \mathcal{F} \), the plug-in estimate \(\hat{\theta} \) of the parameter \( \theta = t(F) \)  of distribution \( F \in \mathcal{F} \) is simply the function \( t(\cdot) \) applied to the empirical distribution \( \hat{\theta} = t(\hat{F}_{n}) \), where the empirical distribution is based on iid data sampled from \( F \), or \( X_{i} \stackrel{iid}{\sim} F \), for \( i = 1, \dots, n \). Note that in this case, the empirical distribution also needs to be an element of the family of distributions \( \mathcal{F} \) that \( t(\cdot) \) is defined on. Probably some examples are in order. 

Lets assume that \( \mu = g(F) = \int_{-\infty}^{\infty}x dF(x) = \mathbb{E}_{F}[X] \) is a parameter we want to estimate. The plug-in principle leads us to the following plug-in estimator: 

$$ \hat{\mu}(x_{1}, \dots, x_{n}) = g(\hat{F}_{n}) = \int_{x \in \{x_{1}, \dots, x_{n}\}} x d\hat{F}_{n}(x) = \sum_{i = 1}^{n} x_{i} \cdot \left(\frac{1}{n}\right) = \frac{1}{n} \sum_{i = 1}^{n} x_{i} = \bar{x} $$
So the plug-in estimator of the expectation of a random variable \( X \) with distribution \( F \), is simply the sample mean of the observed data.

Now let's assume that \( \sigma^{2} = f(F) = \int_{-\infty}^{\infty}(x - \mu)^{2} dF(x) = \mathbb{E}_{F}[(X - \mu)^{2}] = \mathbb{V}ar_{F}[X] \) is another parameter we want to estimate. As before, the plug-in estimator of \( \sigma^{2} \) is:

$$ \hat{\sigma}^{2}(x_{1}, \dots, x_{n}) = f(\hat{F}_{n}) = \int_{x \in \{x_{1}, \dots, x_{n}\}} (x - \hat{\mu})^{2} d\hat{F}_{n}(x) = \sum_{i = 1}^{n} (x_{i} - \bar{x})^{2} \cdot \left(\frac{1}{n}\right) = \frac{1}{n} \sum_{i = 1}^{n} (x_{i} - \bar{x})^{2} = s_{MLE}^{2}(x_{1}, \dots, x_{n}) $$
The plug-in estimator of the variance of a random variable \( X \) with distribution \( F \) is the (biased) maximum likelihood sample variance estimator. This estimator is related to the unbiased sample variance estimator \( s_{U}^{2}(\cdot) \) through the equation: \( \frac{n}{n - 1} s_{MLE}^{2}(\cdot) = s_{U}^{2}(\cdot) = \frac{1}{n - 1} \sum_{i = 1}^{n} (x_{i} - \bar{x})^{2} \). The sample mean \( \bar{x} \) , the sample variances \( s_{MLE}^{2}(x_{1}, \dots, x_{n}) \) and \( s_{U}^{2}(x_{1}, \dots, x_{n})\), are statistical functionals. 

The third important concept is the **bootstrap principle**, which is that when we view a statistical functional \( s(X_{1}, X_{2}, \dots , X_{n}) = s(X) \sim R \) as a random variable defined on iid data (\( X_{i} \stackrel{iid}{\sim} F \)), and we want to be able to find parameters of \( R \), we can do it by applying the plug-in principle. For example, if we are interested in the variance of \( s(X) \), we can denote the variance as the parameter \( \sigma_{s}^{2} = f(R) \); if we somehow could obtain iid data \( t_{1}, \dots, t_{B} \), where \( t_{i} = s(\tilde{X}_{i}) \stackrel{iid}{\sim} R \) and \( \tilde{X}_{i} = (X_{1}, \dots, X_{n}) \), then we could directly estimate \( \sigma_{s}^{2} = f(R) \) with a plug in estimator \( f(\hat{R}_{B}) = \frac{1}{B} \sum_{i = 1}^{B} (t_{i} - \bar{t})^{2} = s_{MLE}^{2}(t_{1}, \dots, t_{B}) \). However, in the situations where we want to use the bootstrap we don't have an iid sample \( t_{1}, \dots, t_{B} \) of the statistic \( s(X) \), we only have the single sample of size one: \( s(x) = s(x_{1}, \dots, x_{n}) \). But we do have the empirical distribution \( \hat{F}_{n} \), and we can sample from it as many times as we desire or our computers will allow. B samples of size n from the empirical distribution \( \hat{F}_{n} \stackrel{iid}{\rightarrow} x^{*(1)}, \dots , x^{*(B)} \) with \( x^{*(b)} = (x_{1}, \dots, x_{n}) \) are called bootstrap samples. It is useful to understand that sampling with replacement from the observed data \( \{x_{1}, \dots, x_{n} \} \) n times is exactly equivalent to sampling n observations from the empirical distribution \( \hat{F}_{n} \). Repeating this process B times results in B bootstrap samples. Applying the statistical functional to each of the bootstrap samples results in bootstrap replications \( t_{1}^{*} = s(x^{*(1)}), \dots, t_{B}^{*} = s(x^{*(B)}) \). The true distribution of the \( t_{i}^{*} \) is not \( R \), because the distribution of the \( x^{*(b)} \) is not the data generating distribution \( F \), but empirical distribution \( \hat{F}_{n} \). We denote the true distribution of \( t_{i}^{*} \) as \( R^{*} \), and we call it the **true bootstrap distribution**. If the empirical distribution \( \hat{F}_{n} \) is close to the true data generating distribution \( F \), then the empirical bootstrap distribution \( \hat{R^{*}_{B}} \) (the empirical distribution of the bootstrap replicates) should be close to the true bootstrap distribution \(R^{*} \). The bootstrap estimator of \( \sigma_{s}^{2} = f(R) \) is simply 

$$ \hat{\sigma}_{s}^{2} = f(\hat{R^{*}_{B}}) = \frac{1}{B} \sum_{i = 1}^{B} (t_{i}^{*} - \bar{t}^{*})^{2} $$
In general, for a functional parameter, defined by \( \lambda(R) \), where \( R \) is the distribution of a statistical functional (as a random variable), the bootstrap estimator of \( \lambda(R) \) is

$$ \lambda(\hat{R^{*}_{B}}) = \int_{t \in \{t^{*(1)}, \dots, t^{*(B)} \}} \nu(t)d\hat{R^{*}_{B}}(t) = \sum_{t \in \{t^{*(1)}, \dots, t^{*(B)} \}} \nu(t) \left(\frac{1}{B}\right) = \frac{1}{B} \sum_{b = 1}^{B} \nu(t^{*}_{b}) = \frac{1}{B} \sum_{b = 1}^{B} \nu(s(x^{*(b)})) $$


Most estimators of parameters involve approximations of some kind, perhaps relying on asymptotic properties like consistency, but bootstrap estimators rely on two separate types of approximations. First, a bootstrap estimator relies on the distribution \( R^{*} \) being close to \( R \) somehow. With the right mathematical argument and appropriate assumptions (way out of scope for us) we could show that \( R^{*} \) is close to \( R \) when the empirical distribution \( \hat{F}_{n} \) is close to \( F \). And second, bootstrap estimators rely on the distribution of \( \hat{R^{*}}_{B} \) being close to \( R^{*} \), which we can generally achieve within desired error bounds by increasing the number of bootstrap replications, B, sufficiently high. Because this is generally true, the accuracy of bootstrap estimators is more constrained by how well \( \hat{F}_{n} \) approximates \( F \) than by how well \( \hat{R^{*}}_{B} \) approximates \( R^{*} \). In other words, the sample size, n, of the observed data is, as always, the strongest determinant of the accuracy of bootstrap estimators. Selecting a really large number of bootstrap replications, B, does little to improve the performance of a bootstrap estimator, especially if the sample size, n, is small.

Because the empirical distribution is known (or in the case of the parametric bootstrap, a fitted distribution is known), we can sample from it as many times as the computer allows for, say B times. Each bootstrap sample \( x^{*(b)} \) (for \( b \in \{ 1, \dots, B\} \)) is an iid sample from the distribution of \( \hat{F}_{n} \), which is simply a random sample of size n with replacement from \( x = ( x_{1}, \dots, x_{n} ) \). By computing \( t^{*(b)} = s(x^{*(b)}) \) for each b, we obtain an iid sample from the distribution of \( t(\hat{F}_{n}) = s(X_{1}, \dots, X_{n}) \), and can use that to estimate a parameter of the distribution of \( t(\hat{F}_{n}) \) using a plug-in estimator (such as \( s_{MLE}^{2}(\cdot) \)) where the distribution of the bootstrap replicates, \( t^{*(1)}, \dots, t^{*(B)} \), is the empirical distribution that we denote by \( \hat{R}_{B}^{*} \). If \( s_{MLE}^{2} = f(\hat{R}_{B}^{*}) \) is a plug-in estimator of the parameter \( f(R^{*}) \) then the true bootstrap distribution is \( R^{*} \). Note that the true bootstrap distribution, \( R^{*} \), is not equal to the distribution \( R \), it is an approximation to it that is only as good as the empirical distribution of \( \hat{F}_{n} \) is as an approximation to \( F \).

# Using the Bootstrap Principle

## Estimating the Standard Error and Bias of an Estimator with the Bootstrap

In the section on the bootstrap principle, the estimation of the variance of a statistical functional was outlined. In this section, the algorithm for the bootstrap estimation of the bias and the standard error of an estimator will be presented.

We assume that the observed data, \( x_{1}, x_{2}, \dots , x_{n} \) are instantiations of the random variables \( X_{1}, X_{2}, \dots , X_{n} \), where \( X_{i} \stackrel{iid}{\sim} F \). Let the statistical functional \( s(x) = s(x_{1}, \dots , x_{n}) \) be an estimator of a parameter \( \theta = g(F) \). An estimator is simply a function of the observed data that we expect to be generally close to some parameter of interest. There are several ways that "close" in this context can be defined, but here we focus on mean squared error.

### Mean Squared Error

The mean squared error of an estimator is a measure of the accuracy of the estimator. The mean squared error of an estimator is the expectation of the squared difference between the estimator and the parameter it is expected to estimate:

$$ \mathrm{MSE}[\hat{\theta}_{n}] = \mathbb{E}_{\theta}\left(\hat{\theta}_{n} - \theta \right)^{2} = \mathbb{V}ar[\hat{\theta}_{n}] + \left( \mathrm{Bias}[\hat{\theta}_{n}] \right)^{2} $$
The mean squared error is composed of the variance and the bias of the estimator. The variance of \( \hat{\theta}_{n} = s(x_{1}, \dots , x_{n}) \) is defined as:

$$ \mathbb{V}ar[\hat{\theta}_{n}] = \mathbb{E}_{\theta}\left(\hat{\theta}_{n} - \mathbb{E}_{\theta}(\hat{\theta}_{n})\right)^{2} $$
And the bias of \( \hat{\theta}_{n} = s(x_{1}, \dots , x_{n}) \) is defined as:

$$ \mathrm{Bias}[\hat{\theta}_{n}] = \mathbb{E}_{\theta} \left( \hat{\theta}_{n} - \theta \right) = \mathbb{E}_{\theta} ( \hat{\theta}_{n}) - \theta $$

\( \mathrm{MSE}[\hat{\theta}] \) is a function of the true value of \( \theta \) and the sample size n, as are both the variance and the bias of the estimator \( \hat{\theta}_{n} \). The smaller the mean squared error the better the estimator can be thought to estimate the quantity of interest, \( \theta \); the closer the estimator tends to be to \( \theta \). 

Of course, the variance and the bias of an estimator are of interest for other reasons: the standard error of an estimator, \( \mathrm{SE}[\hat{\theta}_{n}] \),
is related to the variance in the obvious way, \( \mathrm{SE}[\hat{\theta}_{n}] = \sqrt{\mathbb{V}ar[\hat{\theta}_{n}]} \) and it can be used in the computation of a confidence interval for \( \hat{\theta}_{n} \) or in hypothesis testing; the estimated bias of an estimator could be used to correct the bias of the estimator (not usually a good idea), or sometimes in computing a confidence interval (especially using the bootstrap). Here we show how to find bootstrap estimators of the variance or bias of a given statistical functional that is an estimator.

### Algorithm for Non-parametric Bootstrap Standard Error and Bootstrap Bias

1. Select B independent bootstrap samples \( x^{*(1)}, \dots , x^{*(B)} \) each of size n, drawn with replacement from \( x = \{x_{1}, \dots , x_{n} \} \)
2. Compute the bootstrap replication corresponding to each bootstrap sample \( \hat{\theta^{*}_{n}}(b) = s(x^{*(b)}) \) for each \( b = 1, 2, \dots , B \)
3. Estimate the parameter of interest (variance or bias):

Estimate \( \mathrm{SE}[\hat{\theta}_{n}] \) by the sample standard deviation of the B bootstrap replications:

$$ \hat{\mathrm{SE}}[ \hat{\theta}_{n} ] = \sqrt{ \sum_{i = 1}^{B} \frac{(\hat{ \theta^{*}_{n} }(b) - \hat{ \theta^{*}_{n} }( \cdot ) )^{2}}{B - 1} } $$
Where \( \hat{ \theta^{*}_{n} }( \cdot ) = \frac{1}{B} \sum_{i = 1}^{B} \hat{ \theta^{*}_{n} }(b) \)

Estimate \( \mathrm{Bias}[\hat{\theta}_{n}] \) by:

$$ \hat{\mathrm{Bias}}[\hat{\theta}_{n}] = \frac{1}{B} \sum_{i = 1}^{B} \hat{ \theta^{*}_{n} }(b) - g(\hat{F}) = \hat{ \theta^{*}_{n} }( \cdot ) - \hat{\theta}_{n} $$
There is a slightly superior method for the bootstrap estimation of the bias of an estimator, but we will not cover that here. For more information on that, see pages 130-132 of Bradley Efron and Robert Tibshirani's _An Introduction to the Bootstrap_. 

### Algorithm for Parametric Bootstrap Standard Error and Bootstrap Bias

The parametric bootstrap works in a very similar way to the non-parametric bootstrap when estimating the standard error or bias of an estimator. The primary difference is that the parametric bootstrap does not work by sampling from the empirical distribution of the observed data. One must know which parametric family of distributions the data generating distribution is within. For example, one may know that \( F \) is in the normal family of distributions, but not know which one it is as one may not know the mean \( \mu \) and variance \( \sigma^{2} \) parameters. To form a parametric bootstrap estimate of the statistical functional \( \hat{\theta}_{n} = s(x) \), where \( x = (x_{1}, \dots , x_{n}) \), one must first estimate the parameters of \( F \). If \( F \) is a normal distribution, the statistician is free to find estimates of \( \mu \) and \( \sigma^{2} \) using the best method available. Once estimates \( \hat{\mu} \) and \( \hat{\sigma}^{2} \) have been determined, one now can make samples from the fitted parametric distribution, \( \tilde{F}_{fit} = \mathcal{N}(\hat{\mu}, \hat{\sigma}^{2} ) \), with the use of a computer. Of course, if \( F \) is not the normal distribution, but some other parametric distribution, one needs to properly estimate the parameters of that distribution and determine a fitted distribution for the purposes of the parametric bootstrap. To compute parametric bootstrap estimates of the standard error and bias of an estimator \( \hat{\theta} \):

1. Select B independent bootstrap samples \( x^{*(1)}, \dots , x^{*(B)} \) each of size n, drawn with replacement from the fitted distribution \( \tilde{F}_{fit} \)
2. Compute the bootstrap replication corresponding to each bootstrap sample \( \hat{\theta^{*}_{n}}(b) = s(x^{*(b)}) \) for each \( b = 1, 2, \dots , B \)
3. Estimate the parameter of interest (variance or bias):

Estimate \( \mathrm{SE}[\hat{\theta}_{n}] \) by the sample standard deviation of the B bootstrap replications:

$$ \hat{\mathrm{SE}}[ \hat{\theta}_{n} ] = \sqrt{ \sum_{i = 1}^{B} \frac{(\hat{ \theta^{*}_{n} }(b) - \hat{ \theta^{*}_{n} }( \cdot ) )^{2}}{B - 1} } $$
Where \( \hat{ \theta^{*}_{n} }( \cdot ) = \frac{1}{B} \sum_{i = 1}^{B} \hat{ \theta^{*}_{n} }(b) \)

Estimate \( \mathrm{Bias}[\hat{\theta}_{n}] \) by:

$$ \hat{\mathrm{Bias}}[\hat{\theta}_{n}] = \frac{1}{B} \sum_{i = 1}^{B} \hat{ \theta^{*}_{n} }(b) - g(\hat{F}) = \hat{ \theta^{*}_{n} }( \cdot ) - \hat{\theta}_{n} $$

As you can see, the algorithms for the non-parametric and parametric bootstrap estimates for \( \mathrm{SE}[ \hat{\theta}_{n} ] \) and \( \mathrm{Bias}[\hat{\theta}_{n}] \) are exactly the same after the first step.

### How Many Boostrap Replicates are Needed?

For estimation of the standard error or bias of an estimator, one should use at least 200 bootstrap replications.

# Bootstrap Confidence Intervals

There are many types of confidence intervals that can be computed using bootstrap methods. In this workshop we focus on the following topics related to bootstrap confidence intervals:

* Normal Intervals
* Bootstrap-t Intervals
* Percentile Intervals
* BCa Percentile Intervals
* Bootstrap Calibration


## Normal Theory Confidence Intervals

We first review the basics of confidence intervals for the mean parameter of a normal distribution as it will provide us with notation and terminology important for discussions of bootstrap confidence intervals. Given observed data \( x_{1}, \dots , x_{n} \), assumed sampled independently from the same normal distribution with parameters \( \mu \) and \( \sigma^{2} \), we compute a \( (1 - \alpha) \times 100 \% \) confidence interval for \( \mu \) as:

$$ \left(\bar{x} - t^{\left(1 - \frac{\alpha}{2}\right)} \cdot \frac{\hat{\sigma}}{\sqrt{n}}, \bar{x} + t^{\left(\frac{\alpha}{2}\right)} \cdot \frac{\hat{\sigma}}{\sqrt{n}}\right) $$
Where \( z^{\left( \gamma \right)} \) is a normal \( \gamma \) quantile. We can justify this as a confidence interval because we know the following things are true. 

Since \( X_{i} \stackrel{iid}{\sim} \mathcal{N}(\mu, \sigma^{2}) \), \( \bar{X} \stackrel{iid}{\sim} \mathcal{N}(\mu, \frac{\sigma^{2}}{n}) \), and thus 

$$ Z_{n} = \sqrt{n} \frac{(\bar{X} - \mu)}{\sigma} \stackrel{iid}{\sim} \mathcal{N}(0, 1) $$
When we do not know \( \sigma^{2} \) (pretty much always), then we know that:

$$ T_{n} = \sqrt{n} \frac{(\bar{X} - \mu)}{\hat{\sigma}} \stackrel{iid}{\sim} \mathcal{T}_{n - 1} $$
Where \( \mathcal{T}_{n - 1} \) is the t-distribution with \( n - 1 \) degrees of freedom.

To find a confidence interval for \( \mu \), we can invert a two-sided \( \alpha \)-level hypothesis test with null-hypothesis \( \mathrm{H}_{0}: \mu = \mu_{0} \) and alternative hypothesis \( \mathrm{H}_{1}: \mu \neq \mu_{0} \). This defines an acceptance region for when

$$ t^{(\frac{\alpha}{2})} \leq T_{n} \leq t^{(1 - \frac{\alpha}{2})} $$
And particularly

$$ \mathbb{P}_{\mu_{0}}\left( t^{(\frac{\alpha}{2})} \leq T_{n} \leq t^{(1 - \frac{\alpha}{2})} \right) = \mathbb{P}_{\mu_{0}}\left( t^{(\frac{\alpha}{2})} \leq \sqrt{n} \frac{(\bar{X} - \mu)}{\hat{\sigma}} \leq t^{(1 - \frac{\alpha}{2})} \right) = 1 - \alpha $$
Therefore

$$ \mathbb{P}_{\mu_{0}}\left( t^{(\frac{\alpha}{2})} \leq \sqrt{n} \frac{(\bar{X} - \mu)}{\hat{\sigma}} \leq t^{(1 - \frac{\alpha}{2})} \right) = \mathbb{P}_{\mu_{0}}\left( t^{(\frac{\alpha}{2})} \cdot \frac{\hat{\sigma}}{\sqrt{n}} \leq \bar{X} - \mu \leq t^{(1 - \frac{\alpha}{2})} \cdot \frac{\hat{\sigma}}{\sqrt{n}} \right) =  $$

$$ = \mathbb{P}_{\mu_{0}}\left( \bar{X} - t^{(1 - \frac{\alpha}{2})} \cdot \frac{\hat{\sigma}}{\sqrt{n}} \leq \mu \leq  \bar{X} + t^{(\frac{\alpha}{2})} \cdot \frac{\hat{\sigma}}{\sqrt{n}} \right) = 1 - \alpha $$

This is true for every \( \mu_{0} \in \mathbb{R} \). Thus, \( \left( \bar{x} - t^{(1 - \frac{\alpha}{2})} \cdot \frac{\hat{\sigma}}{\sqrt{n}}, \bar{x} + t^{(\frac{\alpha}{2})} \cdot \frac{\hat{\sigma}}{\sqrt{n}} \right) \) is a \( (1 - \alpha) \times 100 \% \) confidence interval for \( \mu \).

Because the t-distribution is very close to the normal distribution for sample sizes of approximately 30 or greater, it is common for sample sizes larger than 30 to see confidence intervals using normal quantiles in the place of t-quantiles.

## Asymptotically Normal Statistics

The power of the central limit theorem is that given fairly minimal assumptions on data generating distributions, it is possible to prove in many cases that sums of iid random variables are asymptotically normally distributed. When the family of the data generating distribution is known, it is sometimes possible to compute exact confidence intervals, but in other cases, we need to find a good method for an approximate confidence interval. The easiest such method relies on the implicit use of the central limit theorem, which says the following.

### Central limit Theorem
Assuming that \( X_{1}, X_{2}, \dots \) are iid, with expectation \( \mathbb{E}(X_{i}) = \mu \) and finite second moment \( \mathbb{E}(X_{i}^{2}) < \infty \), and that \( S_{n} = \sqrt{n} \frac{(\bar{X} - \mu)}{\sigma} \). Then \( S_{n} \stackrel{d}{\rightarrow} Z \sim \mathcal{N}(0, 1) \) as \( n \rightarrow \infty \). The statistic \( S_{n} \) converges in distribution to the standard normal distribution as the sample size increases without bound.

When the central limit theorem is used to compute confidence intervals it is typical to see normal quantiles used in the place of t-quantiles. 

## A Few Possible Problems with Normal Theory Confidence Intervals

### Non-normality

One reason for the ubiquitous use of confidence intervals employing t-quantiles is that in many cases where there is reason to believe that while the estimator \( \hat{\theta}_{n} \) may be asymptotically normal, in small samples the distribution may be heavier-tailed than a normal distribution. In such a case, using t-quantiles may provide more accurate coverage properties. Heavy tails are one potential problem associated with non-normality. Asymmetry and skewness of the distribution of \( \hat{\theta}_{n} \) is another. A common way of dealing with this type of violation of the normality assumption is to find a monotonic transformation of the observed data, \( \phi = \eta(x) \), that would lead to the distribution of \( \hat{\theta}_{n} \) on the transformed data being closer to a normal distribution. Once a normal theory confidence interval is determined on the transformed scale, the inverse transformation is used to back-transform the confidence interval endpoints, thus resulting in a confidence interval on the original scale. There are no automatic way to find such a transformation, but some common ones are worth trying on a given problem.

### Non-constant Standard Error

Even if the distribution of \( \hat{\theta}_{n} \) is very close to normal, it is sometimes the case that the variance or standard error of \( \hat{\theta}_{n} \) changes appreciably over the range of possible values of \( \theta \). When this happens the single estimate of the standard error used in the normal theory confidence interval may be too small or too large for the particular situation, and may lead to highly inaccurate confidence intervals. As in the case of non-normality of the distribution of \( \hat{\theta}_{n} \), one common method of solving this is to use a transformation. Variance-stabilizing transformations are sought after for a particular situation. There is a systematic approach that can be used in this situation if one can find an estimate of the relationship between the standard error of \( \hat{\theta}_{n} \) and \( \theta \). 

Suppose that \( \sqrt{n} \left( \hat{\theta}_{n} - \theta \right) \stackrel{d}{\rightarrow} \mathcal{N}(0, \sigma^{2}) \), where \( \sigma^{2} = V(\theta) \). Then for a function \( h(x) \), we know that 

$$ \sqrt{n} \left( h(\hat{\theta}_{n}) - h(\theta) \right) \stackrel{d}{\rightarrow} \mathcal{N}(0, \sigma^2 (h^{'}(\theta))^{2}) $$
As could be shown by expanding \( h(\hat{\theta}_{n}) \) in a Taylor's series: \( h(\hat{\theta}_{n}) = h(\theta) + (\hat{\theta}_{n} - \theta)h^{'}(\theta) + o_{p}(\hat{\theta}_{n} - \theta) \). Because \( \sqrt{n} \left( h(\hat{\theta}_{n}) - h(\theta) \right) =  \sqrt{n} (\hat{\theta}_{n} - \theta)h^{'}(\theta) + o_{p} \left(\sqrt{n} ( \hat{\theta}_{n} - \theta) \right) \), and we know that \( \sqrt{n} (\hat{\theta}_{n} - \theta)h^{'}(\theta) \stackrel{d}{\rightarrow} \mathcal{N}(0, \sigma^{2} (h^{'}(\theta))^{2}) \), and for large n \( o_{p} \left(\sqrt{n} ( \hat{\theta}_{n} - \theta) \right) \) is negligible. 

Thus we want to find \( h(x) \) such that \( \sigma^2 (h^{'}(\theta))^{2} = c \), where c is a constant. This leads to the following differential equation:

$$ h^{'}(\theta) = \frac{\partial h}{\partial \theta} = \frac{c}{\sigma} = \frac{c}{\sqrt{V(\theta)}} $$
We can motivate a solution to this differential equation by writing it as: \( \mathrm{d}h = \frac{c}{\sqrt{V(\theta)}} \mathrm{d}\theta \). This can be solved by the following integral:

$$ h(x) = \int_{-\infty}^{x} \frac{c}{\sqrt{V(\theta)}} d\theta = \int_{-\infty}^{x}\frac{c}{\mathrm{SE}[\hat{\theta}_{n}](\theta)} d\theta $$
We can always scale this function to make \( c = 1 \), so we can ignore \( c \) in the above equation. This computation can be performed numerically if the integral is not easy to compute analytically. Note that while we can use this method to find a variance-stabilizing transformation \( h(x) \), in order to use it we must already know the relationship between the parameter, \( \theta \) , being estimated and the standard error of the parameter estimate \( \mathrm{SE}[\hat{\theta}_{n}] \). This can be accomplished by regression sometimes.


### Example of Transformation for Normal Theory Confidence Intervals: Fisher's Transformation

Confidence intervals based on normal approximations (Wald intervals), do not always have good properties. One example of this is in the case when one is estimating a binomial proportion, \( p \), when \( n \cdot p \) and \( n \cdot (1 - p) \) are greater than 10 or so. A classical example is when computing the confidence interval for the correlation coefficient between two random variables \( X \) and \( Y \), where the scale of the data (changing variance over the range of possible values) leads to a confidence interval that is not properly scaled and asymmetrical around the point estimate. Using standard normal theory to compute a confidence interval for the correlation coefficient, \( \rho \), can lead to intervals with undesirable coverage properties. This is why Ronald Fisher introduced what is now called the Fisher transformation.

$$ \phi = \frac{1}{2} \mathrm{ln}\left( \frac{1 + \rho}{1 - \rho} \right) $$
Where \( \rho \) is the pearson correlation. When r is the sample correlation, Fisher was able to show that

$$ \hat{\phi} = \frac{1}{2} \mathrm{ln}\left( \frac{1 + r}{1 - r} \right) \sim \mathcal{N}\left(\phi, \frac{1}{n - 3}\right) $$
Thus, computing a normal confidence interval on the \( \phi \) transformed scale results in a much better confidence interval than computing it on the original data scale. In this case, \( \left( \hat{\phi} - \frac{z^{(1 - \frac{\alpha}{2})}}{\sqrt{n - 3}}, \hat{\phi} + \frac{z^{(\frac{\alpha}{2})}}{\sqrt{n - 3}} \right) \) is a \( (1 - \alpha) \times 100 \% \) confidence interval for \( \phi \). To obtain a confidence interval for \( \rho \), one can simply back-transform the confidence interval endpoints using the inverse transformation, \( \rho = \frac{e^{2 \phi} - 1}{e^{2 \phi} + 1} \),to get

$$ \left( \frac{e^{2 \hat{\phi} - \frac{z^{(1 - \frac{\alpha}{2})}}{\sqrt{n - 3}}} - 1}{e^{2 \hat{\phi} - \frac{z^{(1 - \frac{\alpha}{2})}}{\sqrt{n - 3}}} + 1} , \frac{e^{2 \hat{\phi} + \frac{z^{(\frac{\alpha}{2})}}{\sqrt{n - 3}}} - 1}{e^{2 \hat{\phi} + \frac{z^{(\frac{\alpha}{2})}}{\sqrt{n - 3}}} + 1} \right) $$
Which is a \( (1 - \alpha) \times 100 \% \) confidence interval for \( \rho \) that is much better than the typical normal theory-based confidence interval.

# Normal Confidence Intervals with Bootstrap Standard Error

The simplest, and least useful, type of bootstrap confidence interval is just a normal theory confidence interval, where the estimated standard error is estimated using the bootstrap approach described above, \( \hat{\mathrm{SE}}[ \hat{\theta}_{n} ] \). Assuming \( \hat{\theta}_{n} \) is an estimator for the parameter \( \theta \), we have a normal theory confidence interval

$$ \left( \hat{\theta}_{n} - z^{(1 - \frac{\alpha}{2})} \cdot \hat{\mathrm{SE}}[ \hat{\theta}_{n} ], \hat{\theta}_{n} + z^{(\frac{\alpha}{2})} \cdot \hat{\mathrm{SE}}[ \hat{\theta}_{n} ] \right) $$
This is a \( (1 - \alpha) \times 100 \% \) confidence interval for \( \theta \) that has all the drawbacks of normal theory confidence intervals: frequent poor coverage properties, sensitivity to the normal assumption, and sensitivity to the scale of the data (non-constant standard error). As in all cases, one should perform model diagnostics to check that the normal assumption and possible heteroskedasticity is not too bad when using this type of confidence interval.

Additionally, if it appears warranted (i.e. the bias of the estimator \( \hat{\theta}_{n} \) is significant relative to the estimated standard error), one may also form a \( (1 - \alpha) \times 100 \% \) confidence interval for \( \theta \) as

$$ \left( \hat{\theta}_{n} - \hat{\mathrm{Bias}}[\hat{\theta}_{n}] - z^{(1 - \frac{\alpha}{2})} \cdot \hat{\mathrm{SE}}[ \hat{\theta}_{n} ], \hat{\theta}_{n} - \hat{\mathrm{Bias}}[\hat{\theta}_{n}] + z^{(\frac{\alpha}{2})} \cdot \hat{\mathrm{SE}}[ \hat{\theta}_{n} ] \right) $$

# Studentized Bootstrap-t Confidence Intervals

This method is the first true bootstrap-based approach to computing a confidence interval that we're covering. There are two versions, as is the case for the bootstrap estimates of standard error and bias; there is a non-parametric version and a parametric version. The non-parametric version can be erratic when the sample size \( n \) is not large, so it is not always recommended. However, the parametric version does perform rather well, as long as one is aware that it is scale-dependent like the normal theory confidence intervals, and avoids applying it in situations where the normal approximation to the distribution of \( \hat{\theta}_{n} \) is not good. Finally, this method can be very computationally intensive if a formula for the standard error of \( \hat{\theta}_{n} \) is not known.

Bootstrap confidence intervals computed in this way were not always recommended, primarily due to the observed instability of the non-parametric version in small samples sizes. However, there are theoretical results that indicate that confidence intervals computed in this way are second-order accurate, which is actually more accurate than the usual normal theory confidence intervals, which are first-order accurate. 

The basic takeaway is that when one believes the parametric model is a good fit to the observed data, and the normal approximation to the distribution of the \( t^{*(b)} \) is not bad, this method of computing confidence intervals is usually better than the standard normal theory confidence intervals.

### Parametric Studentized-t Confidence Interval Algorithm

1. Fit a parametric model to the observed data \( x = x_{1}, \dots, x_{n} \) to determine fitted distribution \( \tilde{F}_{fit} \)
2. Generate B bootstrap samples \( x^{*(1)}, \dots , x^{*(B)} \) by sampling n iid observations from \( \tilde{F}_{fit} \) B times
3. Compute B bootstrap replicates \( \hat{\theta^{*}_{n}}^{(1)} , \dots , \hat{\theta^{*}_{n}}^{(B)} \) by applying \( \hat{\theta^{*}_{n}}^{(b)} = s(x^{*(b)}) \) for \( b = 1, \dots , B \)
4. Estimate the standard error \( \mathrm{SE}[ \hat{\theta}_{n} ] \) as \( \hat{\mathrm{SE}}[ \hat{\theta}_{n} ] = \frac{1}{B - 1} \sum_{b = 1}^{B} (\hat{\theta^{*}_{n}}^{(b)} - \hat{\theta^{*}_{n}}(\cdot))^{2} \), where \( \hat{\theta^{*}_{n}}(\cdot))^{2} = \frac{1}{B} \sum_{b = 1}^{B} \hat{\theta^{*}_{n}}^{(b)} \)
5. For each \( \hat{\theta^{*}_{n}}^{(b)} \), generate R bootstrap samples by sampling with replacement n times each from \( x^{*(b)} \), compute R bootstrap replicates \( \hat{\theta^{*}_{n}}^{(1)}(b) , \dots , \hat{\theta^{*}_{n}}^{(R)}(b) \) by applying \( \hat{\theta^{*}_{n}}^{(r)}(b) = s(x^{*(r)}(b)) \) for \( r = 1, \dots , R \), and compute \( \hat{\mathrm{SE}}_{b}[ \hat{\theta}_{n} ] = \frac{1}{R - 1} \sum_{r = 1}^{R} (\hat{\theta^{*}_{n}}^{(r)}(b) - \hat{\theta^{*}_{n}}(b, \cdot))^{2} \), where \( \hat{\theta^{*}_{n}}(b, \cdot))^{2} = \frac{1}{R} \sum_{r = 1}^{R} \hat{\theta^{*}_{n}}^{(r)}(b) \)
6. Compute \( t^{*(b)} = \frac{\hat{\theta^{*}_{n}}^{(b)} - \hat{\theta}_{n}}{\hat{\mathrm{SE}}_{b}[ \hat{\theta}_{n} ]} \) for each \( b = 1, \dots, B \)
7. Compute the \( \alpha \) and \( 1 - \alpha \) quantiles of the \( t^{*(b)} \): \( t^{*}_{((R + 1) \alpha)} \) and \( t^{*}_{((R + 1) (1 -\alpha))} \) - if \( (R + 1) \alpha \) and \( (R + 1) (1 -\alpha)) \) are not integers, interpolation can be used to find the appropriate quantile.

If \( k = (R + 1) \alpha \) is an integer, then \( t^{*}_{((R + 1) \alpha)} \) is the \( k \)th largest value of the \( t^{*(b)} \). Likewise, if \( m = (R + 1) (1 - \alpha) \) is an integer, then \( t^{*}_{((R + 1) (1 - \alpha))} \) is the \( m \)th largest value of the \( t^{*(b)} \).

The \( (1 - \alpha) \times 100 \% \) confidence interval for \( \theta \) is:

$$ \left( \hat{\theta}_{n} - t^{*}_{((R + 1) (1 -\alpha))} \cdot \mathrm{SE}[ \hat{\theta}_{n} ], \hat{\theta}_{n} - t^{*}_{((R + 1) \alpha)} \cdot \mathrm{SE}[ \hat{\theta}_{n} ] \right) $$
Note that the form of the confidence interval is very similar to a normal theory confidence interval, but the way in which \( \hat{\theta}_{n} - t^{*}_{((R + 1) \alpha)} \), \( \hat{\theta}_{n} - t^{*}_{((R + 1) ( 1 - \alpha))} \), and \( \mathrm{SE}[ \hat{\theta}_{n} ] \) are computed is very different. Negative signs are used in the computation of each confidence bound because \( \hat{\theta}_{n} - t^{*}_{((R + 1) \alpha)} \) should be negative, and \( \hat{\theta}_{n} - t^{*}_{((R + 1) ( 1 - \alpha))} \) should be positive. 

It is advisable to create a Q-Q plot of the quantiles of the bootstrap replicates \( t^{*(b)} \) against standard normal quantiles to assess the normal approximation. If the distribution of the \( t^{*(b)} \) does not look very normal, this is a clue that the studentized bootstrap-t interval is not applicable to the situation. 

### Non-parametric Studentized-t Confidence Interval Algorithm

All steps except for the first two are the same as the parametric version. For the non-parametric version, ignore step one above (as there is no parametric distribution to fit), and replace step two with:

2. Generate B bootstrap samples \( x^{*(1)}, \dots , x^{*(B)} \) by sampling n observations with replacement from \( \{ x_{1}, \dots , x_{n} \} \) B times

This results in generating the bootstrap samples from the empirical distribution \( \hat{F}_{n} \) rather than from the fitted distribution \( \tilde{F}_{fit} \). 

### How Many Boostrap Replicates are Needed?

In pretty much any case when computing bootstrap-based confidence intervals, the number of bootstrap replicates, B, must be at least 1000. It is better to have 2000 or more. If one has a formula to estimate the standard error of \( \hat{\theta}_{n} \), one can use that in the place of the bootstrap estimation of standard error \( \hat{\mathrm{SE}}_{b}[ \hat{\theta}_{n} ] \) in step 5. What is necessary is that the estimated standard error of \( \hat{\theta}_{n} \) be calculated for each \( \hat{\theta^{*}_{n}}^{(b)} \), whether it is done with additional bootstrap replicates (computationally expensive) or a formula. In Efron and Tibshirani's _An Introduction to the Bootstrap_, it is recommended that the bootstrap standard error estimation for each \( \hat{\theta^{*}_{n}}^{(b)} \) uses a number of bootstrap replicates, R, that is at least 25. It is probably better to use 100, if possible. Be aware that the total number of bootstrap replicates is \( B \cdot R \), which can be very large when \( B \approx 1000 \) and \( R \approx 100 \): \( B\cdot R \approx 100,000 \).

# Basic Percentile Bootstrap Confidence Intervals

The basic percentile interval is computationally simple, and it does have some useful theoretical properties, but it may not perform well in some situations. This method produces first-order accurate confidence intervals, which is inferior to the accuracy studentized bootstrap-t interval or the BCa percentile interval. However, the basic percentile interval is **transformation-respecting**, which means that in situations such as the computation of a confidence interval for a correlation coefficient where a monotonic transformation is needed to improve normality or variance stability, it typically performs well enough. The basic percentile interval performs _as though_ one found an appropriate transformation, computed a confidence interval on the transformed scale, and then back-transformed the endpoints - if such a transformation exists. Additionally, the basic percentile interval is **range-preserving**, meaning that it does not produce confidence intervals that exceed the bounds of the parameter space corresponding to the parameter \( \theta \) being estimated. 

There are two versions of this type of bootstrap confidence interval, as is the case with all others: non-parametric and parametric. these two versions are only separated by the way that bootstrap samples are generated.

## Bootstrap Cumulative Distribution Function

In order to understand percentile bootstrap methods for computing confidence intervals is is helpful to first be familiar with the bootstrap CDF (cumulative distribution function) \( \hat{G}(t) \). We define the bootstrap CDF as:

$$ \hat{G}(t) = \frac{\# \left\{ \hat{\theta^{*}}^{(b)} \leq t \right\} }{B} = \frac{1}{B} \sum_{b = 1}^{B}\mathbb{I}\left(\hat{\theta^{*}}^{(b)} \leq t\right) $$
The definition of the bootstrap CDF is basically the same as the empirical distribution function, except that the bootstrap CDF is defined using bootstrap replicates, and the empirical distribution is defined using observed data sampled from a data generating distribution.

We denote the percentiles of the bootstrap distribution, \( \hat{\theta^{*}}_{(\alpha)} \) , using the inverse of the bootstrap CDF:

$$ \hat{\theta^{*}}_{(\alpha)} = \hat{G}^{-1}(\alpha) $$
The bootstrap CDF, \( \hat{G}(t) \), can be viewed as an estimator for the cumulative distribution of \( \hat{\theta}_{n} \), but justifying this is not in scope here. Taking this as given, we then can estimate the quantiles of the distribution of \( \hat{\theta}_{n} \) using the quantiles of the bootstrap distribution. Thus, if we denote \( \hat{\theta}_{n}[\alpha] \) as the \( \alpha \) quantile of the distribution of \( \hat{\theta}_{n} \), then we can estimate it as

$$ \hat{\theta}_{n}[\alpha] \approx \hat{\theta^{*}}_{(\alpha)} = \hat{G}^{-1}(\alpha) $$
The median of the distribution of \( \hat{\theta}_{n} \) is thus \( \hat{\theta}_{n}[0.5] \), and the bootstrap estimate of the median of \( \hat{\theta}_{n} \) is \( \hat{\theta^{*}}_{(0.5)} = \hat{G}^{-1}(0.5) \), the middle observation of the bootstrap distribution. 

### Finding Quantiles of the Bootstrap Distribution

To find the \( \alpha \) sample quantile of the bootstrap distribution, we select the kth order statistic (the kth ordered value) where \( k = \alpha (B + 1) \). Given a particular value for \( \alpha \in [0, 1] \), and a bootstrap distribution of B values \( \hat{\theta^{*}_{n}}^{(1)} , \dots , \hat{\theta^{*}_{n}}^{(B)} \), the approximate sample percentile \( k = \alpha (B + 1) \) may not be an integer. For example, if \( B = 1000 \) and \( \alpha = 0.123 \), then \( k = \alpha (B + 1) = 123.5234 \). There is no standard way of determining whether the sample percentile in this case is the 123rd ordered observation or the 124th ordered observation, or some interpolated or weighted average of the two. Choose one that you like and be consistent.

### Non-parametric Basic Percentile Bootstrap Confidence Interval Algorithm

1. Generate B bootstrap samples \( x^{*(1)}, \dots , x^{*(B)} \) by taking a sample of n with replacement from \( x_{1}, \dots , x_{n} \) B times
2. Compute B bootstrap replicates \( \hat{\theta^{*}_{n}}^{(1)} , \dots , \hat{\theta^{*}_{n}}^{(B)} \) by applying \( \hat{\theta^{*}_{n}}^{(b)} = s(x^{*(b)}) \) for \( b = 1, \dots , B \)
3. Order all B bootstrap replicates, and determine the following quantiles of the bootstrap distribution: \( \hat{G}^{-1}(\frac{\alpha}{2}) \) and \( \hat{G}^{-1}(1 - \frac{\alpha}{2}) \)

The \( (1 - \alpha) \times 100 \% \) confidence interval for \( \theta \) is:

$$ \left( \hat{G}^{-1}(\frac{\alpha}{2}), \hat{G}^{-1}(1 - \frac{\alpha}{2}) \right) = \left( \hat{\theta^{*}}_{(\frac{\alpha}{2})}, \hat{\theta^{*}}_{(1 - \frac{\alpha}{2})} \right) $$


### Parametric Basic Percentile Bootstrap Confidence Interval Algorithm

The parametric method for the basic percentile interval differs from the non-parametric method only in the first step. First one needs to know which family of parametric distributions the data generating distribution is in. Then one needs to estimate the parameters of that distribution to obtain the fitted distribution \( \tilde{F}_{fit} \). Once \( \tilde{F}_{fit} \) is determined, then replace step 1 with the following step:

1. Generate B bootstrap samples \( x^{*(1)}, \dots , x^{*(B)} \) by taking a sample of n with replacement from \( \tilde{F}_{fit} \) B times

### How Many Boostrap Replicates are Needed?

It is recommended for B to be at least 2000 when using any percentile interval method. 

# BCa Percentile Bootstrap Confidence Intervals

Despite the nice property of transformation-respecting confidence intervals that the basic percentile bootstrap method has, it does not tend to be as accurate as the studentized bootstrap-t confidence intervals. The basic percentile intervals are only first-order accurate, like usual normal theory confidence intervals, and the non-parametric version can be erratic in sample sizes that are not large. BCa percentile intervals were created to improve on the coverage properties and the accuracy of the basic percentile intervals. BCa intervals perform better than basic percentile intervals because they (1) are corrected for bias and (2) for potential non-constant standard error. These corrections work by choosing bootstrap distribution order statistics as confidence bounds that correspond to different sample percentiles than would be chosen in the basic percentile method.

### Bias Correction of Percentile Intervals

To motivate the way to compute bias corrected percentile intervals, we first make the following assumptions. Assume that given observed data \( x_{1}, \dots , x_{n} \) from a distribution \( F \), we estimate the parameter \( \theta = g(F) \) with the estimator \( \hat{\theta}_{n} = s(x_{1}, \dots , x_{n}) \). Also, assume there is a monotonic transformation \( \phi = h(\theta) \), and that for \( \hat{\phi} = h(\hat{\theta}) \), \( \hat{\phi} \sim \mathcal{N}(\phi, \tau^{2}) \). Then assuming that a percentile bootstrap is conducted on the transformed data \( \hat{\phi^{*}} \sim \mathcal{N}(\phi, \tau^{2}) \), and the \( \alpha \) quantile of the bootstrap distribution \( \hat{G}_{\phi} \) is 

$$ \hat{\phi}_{n}[\alpha] \approx \hat{\phi^{*}}_{(\alpha)} = \hat{\phi} + z^{(\alpha)} \tau $$
A \( (1 - \alpha) \times 100 \% \) confidence interval for \( \phi \) is 

$$ \left( \hat{\phi} - z^{(1 - \frac{\alpha}{2})} \tau, \hat{\phi} - z^{(\frac{\alpha}{2})} \tau \right) = \left( \hat{\phi^{*}}_{(\frac{\alpha}{2})}, \hat{\phi^{*}}_{(1 - \frac{\alpha}{2})} \right) $$
Because \( \hat{\phi^{*}}_{(\frac{\alpha}{2})} = \hat{\phi} + z^{(\frac{\alpha}{2})} \tau = \hat{\phi} - z^{(1 - \frac{\alpha}{2})} \tau \), and \( \hat{\phi^{*}}_{(1 - \frac{\alpha}{2})} = \hat{\phi} + z^{(1 - \frac{\alpha}{2})} \tau = \hat{\phi} - z^{(\frac{\alpha}{2})} \tau \). 

As \( h(\theta) \) is monotonic so is its inverse, and applying it to the ordered values of \( \hat{\phi^{*(b)}} \) does not alter the order, resulting in the ordered values of \( \hat{\theta^{*(b)}} \). Therefore 

$$ \left( h^{-1}(\hat{\phi^{*}}_{(\frac{\alpha}{2})}), h^{-1}(\hat{\phi^{*}}_{(1 - \frac{\alpha}{2})}) \right) = \left( \hat{\theta^{*}}_{(\frac{\alpha}{2})}, \hat{\theta^{*}}_{(1 - \frac{\alpha}{2})} \right) $$
This demonstrates the transformation respecting property of percentile intervals. 

However, bias corrected bootstrap percentile confidence intervals are derived using somewhat different assumptions. In particular, as is the case with basic percentile intervals, a normalizing transformation \( \phi = h(\theta) \) is assumed to exist. We assume that

$$ \hat{\phi} - \phi \sim \mathcal{N}(-z_{0}, \sigma^{2}) $$
And we also assume that 

$$ \hat{\phi^{*}} - \hat{\phi} \sim \mathcal{N}(-z_{0}, \sigma^{2}) $$
Let \( \hat{G}_{\phi}(\phi) \) be the bootstrap replicate cumulative distribution on the \( \phi \) scale, and let \( \hat{G}_{\theta}(\theta) \) be the bootstrap replicate cumulative distribution on the \( \theta \) scale.

Note that 

$$ \hat{G}_{\phi}(\hat{\phi}) = \hat{G}_{\phi}(h(\hat{\theta})) = \hat{G}_{\theta}(\hat{\theta}) $$
Then also

$$ \hat{G}_{\theta}(\hat{\theta}) = \hat{G}_{\phi}(\hat{\phi}) = \mathbb{P}(\hat{\phi^{*}} \leq \hat{\phi}) = \mathbb{P}\left(\frac{\hat{\phi^{*}} - \hat{\phi}}{\sigma} \leq 0\right) = \mathbb{P}\left(\frac{\hat{\phi^{*}} - \hat{\phi}}{\sigma} + z_{0} \leq z_{0} \right) = \Phi(z_{0}) $$
Thus, with this, we can express \( z_{0} \), a measure of the bias of the estimator \( \hat{\theta}_{n} \) on the normal scale, estimated using the distribution of bootstrap replicates, as

$$ z_{0} = \Phi^{-1}(\hat{G}(\hat{\theta})) = \Phi^{-1}\left(\frac{\# \left\{ \hat{\theta^{*}}^{(b)} \leq \hat{\theta}_{n} \right\} }{B}\right) $$
Since \( \mathbb{P}(\hat{\phi^{*}} \leq \hat{\phi}) = \Phi^{-1}\left(\frac{\# \left\{ \hat{\theta^{*}}^{(b)} \leq \hat{\theta}_{n} \right\} }{B}\right) \).

Because \( \frac{\hat{\phi} - \phi}{\sigma} + z_{0} \sim \mathcal{N}(0, 1) \) and \( \frac{\hat{\phi^{*}} - \hat{\phi}}{\sigma} + z_{0} \sim \mathcal{N}(0, 1) \), we can form a \( (1 - \alpha) \times 100 \% \) confidence interval on the \( \phi \) scale by observing that 

$$ \mathbb{P}(z^{(\frac{\alpha}{2})} \leq \frac{\hat{\phi} - \phi}{\sigma} + z_{0} \leq z^{(1 - \frac{\alpha}{2})} ) = \mathbb{P}(\hat{\phi} + z_{0}\sigma - z^{(1 - \frac{\alpha}{2})} \sigma \leq \phi \leq \hat{\phi} + z_{0}\sigma - z^{(\frac{\alpha}{2})}\sigma ) = 1 - \alpha $$
Note that

$$ \hat{G}_{\phi}(\hat{\phi} + z_{0}\sigma - z^{(1 - \frac{\alpha}{2})} \sigma) = \mathbb{P}(\hat{\phi^{*}} \leq \hat{\phi} + z_{0}\sigma - z^{(1 - \frac{\alpha}{2})} \sigma) = \mathbb{P}\left( \frac{\hat{\phi^{*}} - \hat{\phi}}{\sigma} + z_{0} \leq 2z_{0} - z^{(1 - \frac{\alpha}{2})} \right) = \Phi(2z_{0} - z^{(1 - \frac{\alpha}{2})}) $$
Similarly, 

$$ \hat{G}_{\phi}(\hat{\phi} + z_{0}\sigma - z^{(\frac{\alpha}{2})} \sigma) = \Phi(2z_{0} - z^{(\frac{\alpha}{2})}) $$
Since \( \hat{G}_{\phi}(\hat{\phi}) = \hat{G}_{\theta}(\hat{\theta}) \), and \( h^{-1}\left( \hat{G}^{-1}_{\phi}(\hat{G}_{\phi}(\hat{\phi})) \right) = \hat{\theta} \) we know that \( \hat{\theta} = h^{-1}\left( \hat{G}^{-1}_{\phi}(\hat{G}_{\theta}(\hat{\theta})) \right) \), and it thus follows that for every \( p \in [0, 1] \)

$$ h^{-1}\left( \hat{G}^{-1}_{\phi}(p) \right) = \hat{G}^{-1}_{\theta}(p) $$
With this result we can back-transform the confidence interval endpoints from the \( \phi \) scale to the \( \theta \) scale to obtain a confidence interval on the \( \theta \) scale. For the lower confidence interval endpoint we get

$$ h^{-1}\left( \hat{G}^{-1}_{\phi}(\Phi(2z_{0} - z^{(1 - \frac{\alpha}{2})})) \right) = \hat{G}^{-1}_{\theta}\left( \Phi(2z_{0} - z^{(1 - \frac{\alpha}{2})}) \right) $$
And for the upper confidence interval endpoint we get

$$ h^{-1}\left( \hat{G}^{-1}_{\phi}(\Phi(2z_{0} - z^{(\frac{\alpha}{2})})) \right) = \hat{G}^{-1}_{\theta}\left( \Phi(2z_{0} - z^{(\frac{\alpha}{2})}) \right) $$
Therefore, a \( (1 - \alpha) \times 100 \% \) bias corrected percentile bootstrap confidence interval for \( \theta \) is:

$$ \left( \hat{\theta}_{BC}\left[\frac{\alpha}{2}\right], \hat{\theta}_{BC}\left[1 - \frac{\alpha}{2}\right] \right) = \left( \hat{G}^{-1}_{\theta}\left( \Phi(2z_{0} - z^{(1 - \frac{\alpha}{2})}) \right), \hat{G}^{-1}_{\theta}\left( \Phi(2z_{0} - z^{(\frac{\alpha}{2})}) \right) \right) $$
Note that when \( z_{0} = 0 \) this reduces to the usual basic percentile confidence interval. 

The bias corrected percentile confidence interval is an improvement on the basic percentile confidence interval, but it is not sufficient to substantially increase the accuracy of such intervals. For that, it is helpful to also include an adjustment that deals with estimators with standard error that changes over the range of the parameter being estimated. This is where the acceleration comes in. 

### Bias Corrected and Accelerated Percentile Intervals

The BCa percentile interval was created by first postulating the existence of a monotone transformation \( \phi = h(\theta) \) that results in a less restrictive distributional assumption than simple normality. It is assumed that this transformation results in 

$$ \hat{\phi} \sim \mathcal{N}(\phi - z_{0}\tau_{\phi}, \tau^{2}_{\phi}) $$
Where \( \tau_{\phi} = 1 + a\phi \). The constant \( a \) is a generally small constant that provides information about how quickly \( \mathrm{SE}[ \hat{\phi}_{n} ] \) varies over the range of \( \phi \). One drawback to the BCa approach is that the acceleration, a, is not a function of the bootstrap distribution. Thus, it must be calculated by other means than using the bootstrap distribution and the bootstrap replicates. There are several ways to compute estimates of the acceleration, but we will focus on one method that can be used for univariate parameters. 

In general, the acceleration is proportional to the skewness of the score function (derivative of the log-likelihood function). But when the distribution \( F \) is unknown, the likelihood is not known, and it cannot be used to compute the acceleration. The simplest way to compute the acceleration is the following method based on the jackknife values of the estimator:

$$ a = \frac{1}{6} \frac{\sum_{i = 1}^{n} (\hat{\theta}_{(\cdot)} - \hat{\theta}_{(i)})^{3}}{\left(\sum_{i=1}^{n} (\hat{\theta}_{(\cdot)} - \hat{\theta}_{(i)})^{2}\right)^{\frac{3}{2}}} $$
Where \( \hat{\theta}_{(i)} = s(x_{(i)}) \), in which case the notation \( x_{(i)} \) is not intended to indicate an order statistic, or the ith observation in the data, but the observed data \( x = (x_{1}, \dots , x_{n}) \) with the ith observation left out: i.e. \( x_{(i)} = (x_{1}, x_{2}, \dots, x_{i-1}, x_{i+1}, \dots , x_{n}) \). Thus, \( \hat{\theta}_{(i)} = s(x_{(i)}) \) is the estimator value with the ith data observation left out. 

The definition of \( \hat{\theta}_{(\cdot)} \) is

$$ \hat{\theta}_{(\cdot)} = \frac{1}{n} \sum_{i=1}^{n} \hat{\theta}_{(i)} $$
Which is the average estimator value when one observation is left out.

### Non-parametric BCa Percentile Bootstrap Confidence Interval Algorithm

1. Generate B bootstrap samples \( x^{*(1)}, \dots , x^{*(B)} \) by taking a sample of n with replacement from \( x_{1}, \dots , x_{n} \) B times
2. Compute B bootstrap replicates \( \hat{\theta^{*}_{n}}^{(1)} , \dots , \hat{\theta^{*}_{n}}^{(B)} \) by applying \( \hat{\theta^{*}_{n}}^{(b)} = s(x^{*(b)}) \) for \( b = 1, \dots , B \)
3. Order the B bootstrap replicates to obtain the bootstrap CDF \( \hat{G}(t) \), and then compute \( z_{0} = \Phi^{-1}(\hat{G}(\hat{\theta})) \)
4. Using the jackknife-based method, compute the acceleration \( a = \frac{1}{6} \frac{\sum_{i = 1}^{n} (\hat{\theta}_{(i)} - \hat{\theta}_{(\cdot)})^{3}}{\left(\sum_{i=1}^{n} (\hat{\theta}_{(i)} - \hat{\theta}_{(\cdot)})^{2}\right)^{\frac{3}{2}}} \)
5. Determine lower and upper percent points \( \alpha_{1} = \Phi\left( z_{0} + \frac{z_{0} + z^{(\frac{\alpha}{2})}}{1 - a \left(z_{0} + z^{(\frac{\alpha}{2})}\right)} \right) \) and \( \alpha_{2} = \Phi\left( z_{0} + \frac{z_{0} + z^{(1 - \frac{\alpha}{2})}}{1 - a \left(z_{0} + z^{(1- \frac{\alpha}{2})}\right)} \right) \)
3. Order all B bootstrap replicates, and determine the following quantiles of the bootstrap distribution: \( \hat{G}^{-1}(\alpha_{1}) \) and \( \hat{G}^{-1}(\alpha_{2}) \)

The \( (1 - \alpha) \times 100 \% \) BCa percentile confidence interval for \( \theta \) is:

$$ \left( \hat{G}^{-1}(\alpha_{1}), \hat{G}^{-1}(\alpha_{2}) \right) = \left( \hat{\theta^{*}}_{(\alpha_{1})}, \hat{\theta^{*}}_{(\alpha_{2})} \right) $$

### Parametric BCa Percentile Bootstrap Confidence Interval Algorithm

Like the basic percentile interval, the parametric method for the BCa percentile interval differs from the non-parametric method only in the first step. Assuming that one knows which family of parametric distributions the data generating distribution is in, then one needs to estimate the parameters of that distribution to obtain the fitted distribution \( \tilde{F}_{fit} \). Once \( \tilde{F}_{fit} \) is determined, then replace step 1 with the following step:

1. Generate B bootstrap samples \( x^{*(1)}, \dots , x^{*(B)} \) by taking a sample of n with replacement from \( \tilde{F}_{fit} \) B times

All other steps proceed in the same way. However, when the distribution of \( F \) is known, it is possible to compute the acceleration in a different way from what we present here. This is out of scope for this workshop, but if you are interested in pursuing this further, see _Bootstrap methods and the Application_, by Davison and Hinkley, pages 203-210.

### How Many Boostrap Replicates are Needed?

It is recommended for B to be at least 2000 when using any percentile interval method. 

# Bootstrap Calibration of Confidence Interval Bounds

### Confidence Point Calibration Algorithm

1. Generate B bootstrap samples \( x^{*(1)}, \dots , x^{*(B)} \) by taking a sample of n with replacement from \( x_{1}, \dots , x_{n} \) B times
2. Compute \( \lambda \)-level confidence points \( \hat{\theta^{*}}_{\lambda}(b) \) for a grid of values of \( \lambda \). These could be computed using any confidence interval method.
3. For each value of \( \lambda \) compute \( \hat{p}(\lambda) = \frac{\# \left( \hat{\theta} \leq \hat{\theta^{*}}_{\lambda}(b) \right) }{B} \)
4. Find the value of \( \lambda \) satisfying \( \hat{p}(\lambda) = \alpha \)

# What if the Data are not iid?

There are several ways that data we would like to analyze in real situations may not be iid

* Two or multiple sample situations
    + Independent samples
    + Dependent data: correlation or regression analysis
* Data with repeated measures or technical replicates
* Time series data, where there is dependence between different time points

**Q**: Can we use bootstrap methods in these situations?

**A**: In many cases, yes, but we need to be careful how we do it

### Two or Multiple Sample Situations - Independent Samples

This is the easiest case of non-iid data. In this case, one simply needs to create bootstrap samples from each independent data set, and then compute the bootstrap replicates as one would to compute \( \hat{\theta} \).

(More needs to be added here)




# Examples

## Normal Distribution Simulations

In order to assess how well bootstrap estimates of bias and standard error perform, we simulate data from a normal distribution with mean 20 and variance 25 (SD 5). We create 7 simulations data sets, by sampling different numbers of observations:

#### Sample Sizes

* 10
* 25
* 50
* 75
* 100
* 1,000
* 10,000

### How Accurately does the Empirical Distribution Estimate the True Normal CDF?

The following plots provide a graphical indication of how the empirical distribution function becomes more and more like the true cumulative distribution function of the normal(20, 25) distribution.

![](Bootstrapping_Workshop_files/figure-latex/cumulative_distribution_functions-1.pdf)<!-- --> ![](Bootstrapping_Workshop_files/figure-latex/cumulative_distribution_functions-2.pdf)<!-- --> 

These graphs display clear evidence that just as we'd expect with any estimator, increasing sample size leads to an increasing accurate estimate. The density estimates (made with Gaussian-kernal) are especially good in this case, allowing for the eye to pick up on differences that are difficult to see when looking at the cumulative density functions. However, this may not always be the case. Density estimation is rather difficult to do well, and when the underlying data are very non-normal, and especially when the support is bounded (e.g. positive values only), Gaussian-kernel density estimation may not perform as well.

### Estimates of the Mean and Variance

In order to observe the effect that increasing sample size has on the closeness of the point estimates of the true mean and variance of the normal distribution from which the data were sampled, look at the following table.



Table: Sample Estimates on Normal(20, 25) Data

|Sample Size |Sample Mean |Sample Variance (UNB) |Sample Variance (MLE) |
|:-----------|:-----------|:---------------------|:---------------------|
|10          |21.929      |12.109                |10.898                |
|25          |18.946      |30.691                |29.463                |
|50          |20.525      |28.927                |28.348                |
|75          |19.369      |23.420                |23.108                |
|100         |20.001      |31.179                |30.868                |
|1000        |20.099      |26.496                |26.470                |
|10000       |20.026      |25.306                |25.303                |



### Bootstrap Estimation of Bias and Standard Error of the MLE Sample Variance

We perform four different types of bootstrap estimation to obtain four different estimates of bias and standard error of the biased sample variance estimator (the maximum likelihood estimator), which we compare to their theoretical values. 

The four different types of bootstrap are:

1. Parametric with 200 bootstrap replicates
2. Parametric with 2000 bootstrap replicates
3. Non-parametric with 200 bootstrap replicates
4. Non-parametric with 2000 bootstrap replicates

The following scatterplots of bias and standard error estimates plotted versus the true theoretical values indicate that regardless of which bootstrap is used, as long as 200 or more bootstrap replicates are used, it is actually the sample size that displays an association with the estimates being closer to their true values, as we should expect.

These scatterplots provide some reason to think that the parametric bootstrap bias estimate may be upwardly biased. Also, it appears that all of the standard error estimates for bot non-parametric and parametric bootstrap methods are downwardly biased. Remember that even though the bootstrap variance estimate of the sample variance estimator employs the unbiased sample variance estimator, taking the square root of the variance estimate is necessary to obtain the standard error. The downward bias is introduced by the square root (as an argument employing Jensen's inequality can show), most likely. This teaches a good lesson: do not assume that even the most commonly used statistics are not biased. By the way, it is harder to find an unbiased estimator of the standard deviation than it is to find an unbiased estimator of variance, since unbiased estimators for standard error are different from distribution to distribution. 

![](Bootstrapping_Workshop_files/figure-latex/parametric_bootstrap_bias_estimation-1.pdf)<!-- --> ![](Bootstrapping_Workshop_files/figure-latex/parametric_bootstrap_bias_estimation-2.pdf)<!-- --> 

The following table displays the actual numerical values of the estimates of bias and standard error of the MLE sample variance estimator, for each of the bootstrap methods.



Table: Comparison of Bias and SE Estimates to True Values

|Sample Size |Bootstrap Type     |True Bias |Bias Estimate |True Standard Error |SE Estimate |
|:-----------|:------------------|:---------|:-------------|:-------------------|:-----------|
|10          |parametric 200     |-2.500    |0.228         |16.260              |4.962       |
|25          |parametric 200     |-1.000    |0.016         |10.822              |9.020       |
|50          |parametric 200     |-0.500    |0.191         |7.779               |5.577       |
|75          |parametric 200     |-0.333    |0.275         |6.386               |3.947       |
|100         |parametric 200     |-0.250    |0.509         |5.545               |4.917       |
|1000        |parametric 200     |-0.025    |-0.053        |1.766               |1.077       |
|10000       |parametric 200     |-0.002    |-0.019        |0.559               |0.365       |
|10          |nonparametric 200  |-2.500    |-1.334        |16.260              |3.104       |
|25          |nonparametric 200  |-1.000    |-0.604        |10.822              |8.306       |
|50          |nonparametric 200  |-0.500    |-0.992        |7.779               |5.235       |
|75          |nonparametric 200  |-0.333    |-0.325        |6.386               |3.745       |
|100         |nonparametric 200  |-0.250    |-0.154        |5.545               |3.876       |
|1000        |nonparametric 200  |-0.025    |0.066         |1.766               |1.203       |
|10000       |nonparametric 200  |-0.002    |-0.010        |0.559               |0.339       |
|10          |parametric 2000    |-2.500    |0.069         |16.260              |5.232       |
|25          |parametric 2000    |-1.000    |-0.191        |10.822              |8.525       |
|50          |parametric 2000    |-0.500    |-0.151        |7.779               |5.726       |
|75          |parametric 2000    |-0.333    |-0.062        |6.386               |3.819       |
|100         |parametric 2000    |-0.250    |0.005         |5.545               |4.542       |
|1000        |parametric 2000    |-0.025    |0.000         |1.766               |1.160       |
|10000       |parametric 2000    |-0.002    |-0.002        |0.559               |0.350       |
|10          |nonparametric 2000 |-2.500    |-1.206        |16.260              |3.371       |
|25          |nonparametric 2000 |-1.000    |-1.357        |10.822              |7.801       |
|50          |nonparametric 2000 |-0.500    |-0.497        |7.779               |5.455       |
|75          |nonparametric 2000 |-0.333    |-0.341        |6.386               |3.679       |
|100         |nonparametric 2000 |-0.250    |-0.411        |5.545               |3.810       |
|1000        |nonparametric 2000 |-0.025    |-0.041        |1.766               |1.245       |
|10000       |nonparametric 2000 |-0.002    |-0.002        |0.559               |0.356       |




It's somewhat remarkable that the bootstrap replicate density estimates are all so similar between the non-parametric and the parametric methods in this case. Note that when estimating the standard error and the bias of the sample variance estimator, the performance of the bootstrap is equivalent for 200 bootstrap replicates to when 2000 bootstrap replicates are used.

![](Bootstrapping_Workshop_files/figure-latex/boostrap_replicate_densities-1.pdf)<!-- --> 


# Recommended Reading

#### Books

* Bradley Efron and Robert J. Tibshirani, An Introduction to the Bootstrap. 
* A.C. Davison and D.V. Hinkley, Bootstrap Methods and their Application.
* Bradley Efron and Trevor Hastie, Computer Age Statistical Inference.
* Peter Hall, The Bootstrap and Edgeworth Expansion.
* Bradley Efron, The Jackknife, the Bootstrap and Other Resampling Plans.
* E.L. Lehmann, Elements of Large Sample Theory.
* Keith Knight, Mathematical Statistics. 
* Peter J. Bickel and Kjell A. Doksum, Mathematical Statistics: Basic Ideas and Selected Topics, vol 1.
* Peter J. Bickel and Kjell A. Doksum, Mathematical Statistics: Basic Ideas and Selected Topics, vol 2.
* George Casella and Roger L. Berger, Statistical Inference.
* A.W. van der Vaart, Asymptotic Statistics. 
* Aad W. van der Vaart and Jon A. Wellner, Weak Convergence and Empirical Process Theory.

### Journal Articles

* Efron (1987), Better Bootstrap Confidence Intervals. Journal of the American Statistical Association, 82:397, 171-185
* DiCiccio and Efron (1996), Bootstrap Confidence Intervals. Statistical Science, vol. 11, no. 3, 189-228
* Tibshirani (1984), Bootstrap Confidence Intervals. Department of Statistics, Stanford University, Technical Report No. 3.
* Efron (2003), Second Thoughts on the Bootstrap. Statistical Science, vol. 18, no. 2, 135-140
* P.J. Bickel and E.L. Lehmann (1969), Unbiased Estimation in Convex Families. The Annals of Mathematical Statistics, vol. 40, no. 5, 1523-1535
