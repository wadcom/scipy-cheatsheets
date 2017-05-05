# scipy.stats

Reference:

 * [Continuous distributions](https://docs.scipy.org/doc/scipy/reference/stats.html#continuous-distributions)
 * [Discrete distributions](https://docs.scipy.org/doc/scipy/reference/stats.html#discrete-distributions)

## Normal Distribution

 * [Documentation](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.norm.html)
 * `loc`: mean
 * `scale`: standard deviation

Example: cat weighs are normally distributed with mean of 15lbs and standard
deviation 3lbs. What's the probability that a random cat weighs below 10lbs?

    >>> from scipy.stats import norm
    >>> norm.cdf(10, loc=15, scale=3)
    0.047790352272814703

## Binomial Distribution

 * [Documentation](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.binom.html)

Example: in 100 trials with a coin we got 52 heads. What is the probability
that in next 100 trials we'll get less than 45 heads?

    >>> p = 52 / 100
    >>> from scipy.stats import binom
    >>> binom.cdf(45, 100, p)
    0.096653350327769116

## Poisson Distribution

 * [Documentation](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.poisson.html)
 
Example: on average 5.42 customers visit a bank per hour. What is the probability that exactly
0 customers visit the bank in a given hour?

    >>> poisson.pmf(0, 5.42)
    0.0044271466478315114

What is the probability that 3 or less customers visit the bank?

    >>> poisson.cdf(3, 5.42)
    0.21093087668119684

## Uniform Distribution

 * [Documentation](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.uniform.html)
 * `loc`: left border of the interval
 * `scale`: width of the interval

Example: the airplane leaves any moment between 10:10 and 10:30 with equal
probability. What is the probability that a person who arrived at 10:25 will
make it to the plane?

    >>> from scipy.stats import uniform
    >>> 1 - uniform.cdf(25, 10, 20)
    0.25

## Geometric Distribution
 
 * [Documentation](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.geom.html)

Example: a basketball player throws a ball with 54% accuracy. What is the
probability that he hits the basket 3 times in a row?

Note: the "success" here is missing the target. We need the probability that
the *4th* throw will be a "success".

    >>> from scipy.stats import geom
    >>> geom.pmf(4, 1 - 0.54)
    0.072433440000000002

## Exponential Distribution

 * [Documentation](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.expon.html)
 * `scale = 1 / lambda`

Example: the average lifetime of a laptop computer is 4 years. What is the
probability that the computer will break between 2 and 3 years of use?

    >>> from scipy.stats import expon
    >>> scale = 1 / 4
    >>> expon.cdf(3, scale=scale) - expon.cdf(2, scale=scale)
    0.00032931841554917352

## Beta Distribution

 * [Documentation](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.beta.html)
 * Intuition behind Beta distribution: [StackExchange answer](https://stats.stackexchange.com/questions/47771/what-is-the-intuition-behind-beta-distribution/47782#47782)
 * `loc` and `scale` are not used in typical applications

Example: a baseball player typically hits the target in 27% of the cases ("his
batting average is 0.27"). Also the range of _batting_ _averages_ (e.g.
probabilities of hitting the target) is typically from .21 to .35. If in the
new season the player hits the target in 40 of 100 attempts, what is the
probability that his batting average _at_ _the_ _end_ of this season will be
over .30?

Before the new season starts, the distribution of batting averages is a beta
distribution with parameters:

    >>> alpha = 81
    >>> beta = 219

These parameters are hand-picked, so that:

    >>> float(alpha) / (alpha + beta)
    0.27

...and majority of the distribution lies between .21 and .35:
![Beta(81, 219)](https://i.stack.imgur.com/RJDrz.png)

At the end of the season, the player's batting average distribution will be
a beta distribution with new parameters:

    >>> hits = 40
    >>> attempts = 100
    >>> misses = attempts - hits
    >>> new_alpha = alpha + hits
    >>> new_beta = beta + misses
    >>> new_alpha, new_beta
    (121, 279)

Now we have to calculate the probability of batting average over 30%:

    >>> from scipy.stats import beta
    >>> 1 - beta.cdf(0.3, new_alpha, new_beta)
    0.537688086104126
