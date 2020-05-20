As I was unable to locate number of hypothesis tests and other statistical functions on python or scipy.stats, I have made these functions to assist in data analytics. The names and brief explanation of each and found below.

-These functions were defined in Python3. NumPy and scipy.stats functions norm and t are necessary to run these. This was because these functions are designed to assist me specifically when running these modules anyway, as their purpose is to offer statistical inference on datasets.

# FUNCTIONS

*Note: for the vast majority of functions, the confidence argument defaults at 95% (0.95), as this is a very common desired confidence level. This can be changed at any time passing another value into confidence in those relevant functions. 

1) two_tail(phat)
This function is primarily used within other functions and not often likely to need to be called. This function takes a one-tail normal distribution proportion (such as
a z-score), and returns a two-tail proportion instead. Used when converting a desired confidence level into a z* critical value for
confidence intervals. 

2) tstar(conf_level, n)
Typically, tstar() is used within the other functions. Input your desired confidence level and sample size, and this function will return an appropriate critical value for calculating a confidence interval for a sample distribution of the sample mean. The correct degrees of freedom (n-1) is automatically calculated, so be sure to pass the actual sample size (often len(list) or np.sum(array) depending on data being measured).

3) z_sample_size_calc(phat = 0.5, margin_error = 0.02, confidence = 0.95)
This function calculates a minimum sample size needed to achieve a given confidence level and margin of error for the interval. All arguments have default values, so the function will run with no arguments passed if needed. In cases where a sample proportion is not actually known, a conservative estimate is defaulted (phat = 0.5), as this will provide the largest sample size, to be certain of appropriate minimum size. Returns an integer value n (rounding up to float value +1). 

4) t_sample_size_calc(stdev, margin_error, confidence)
As opposed to z_sample_size_calc, no default arguments (except for confidence) are passed in this function as there is more variability in potential arguments. If a known or estimated standard deviation and desired confidence level and margin of error are passed, t_sample_size_calc will return the minimum sample size needed to appropriately measure a confidence interval of a mean with the desired confidence and range. Very useful when planning potential a/b tests and/or surveys, etc. Note that a z* critical value is used, as t* is not useable when n is not known.

5) z_conf_intv(confidence, phat, n, confidence) 
z_conf_intv takes a desired confidence level (as a decimal), a sample proportion and a sample size, and returns a confidence level as
(min, max). This function automatically converts your desired confidence level into an appropriate z* critical value. The returned value, therefore, gives you a (confidence)% certainty that the true population proportion is captured in the range (min, max). 

6) t_conf_intv(bar, stdev, n, confidence)
Using a z* score when inferring a sample mean is not appropriately accurate, and a t score is used instead. This function works the same as z_conf_intv, but is used specifically when inferring a sample mean vs a sample proportion. The sample standard deviation must also be passed, in addition to the sample mean and sample size. Returns a (min, max) range.


7) diff_t_intv(sample1, sample2, confidence)
This function takes two samples entirely (as list or array type), as well as a default confidence argument. The sample means, sample standard deviations and sample sizes are automatically calculated from these datasets. The resulting (min, max) value returned infers, at a given confidence%, that the true difference of mean between population1 and population2 is captured in this range. Don't forget about hypothesis testing! H0: xbar1-xbar2 = 0, so if 0 is included in interval, you fail to reject H0 at a preset alpha = 1-confidence.

8) one_samp_z_test(phat, p0, n, both_tails = False, alpha = None)
Calculates the probability of getting a given sample proportion (phat) with n samples, assuming null hypothesis proportion (p0) is true. Enter phat, p0 and n, and you will get a return of the z-score and the p-value of getting that extreme or more of a score away from p0. Note that this means that if a z_score is positive, this function automatically calculates probability of given ratio OR GREATER, as opposed to using a z-table manually. Be mindful of this if you intend to calculate that value or less, or to prevent accidentally reversing an appropriate probability. If your alternative hypothesis is that p ≠ p0, vs p > or < p0, pass both_tails = True to get a more appropriate p-value for your hypothesis test. If you enter an alpha value - by passing alpha = 0.5 for example - it will return "reject" as True/False.
In this case, you will get 3 return values instead of 2 (z statistic, p-value, and a reject = True/False). This defaults to None, in which case this function returns only a z statistic and a p-value. 

9) two_samp_z_test(sample1, sample2, both_tails = False)
Takes two sample values as list/array type of raw numbers (# of successes k, # of failures = total n), and measures their proportions to tests H0: true p1 = true p2, which is a test of significance between two sample proportions. The sample list CAN contain more than one item of failures, but only 1 index (index 0) for successes, in order to calculate proportion. In other words, success k should be the first item in each list. Returns a z_statistic and p-value of both sample proportions coming from the same true p. If both_tails = True (when Ha: p1 != p2, vs > or < p2), the returned p-value is doubled to reflect it. Returns the z_stat and the p-value. Note: This calculates the p-val of getting a z_score as extreme or more automatically - if your z_score is positive, it automatically returns the 1 - z_score ratio (probability of getting a z-score that high or HIGHER, as opposed to what the z-table would calculate).

10) expected(pop_lists)
The scipy.stats module has two excellent chi-square statistic based hypothesis tests, chisquare() and chi2_contingency(). The first tests for goodness of fit, and assumes equally likely expected outcomes (evenly distributed). The second measures a contingency probability, meaning that the data comes from the same population and is testing potential correlation between multiple parameters.. chisquare() is appropriate for also doing a chi squared homogeneity test specifically (testing between 2 separate populations for similarity), assuming that you know what the "expected" values would be based on your samples. That is where this function comes in. Each index in pop_lists should be a list of one population's data. So if you are comparing 3 populations, pop_lists will be a list of 3 lists - [[pop1 data], [pop2 data], [pop3 data]] for example. In these cases where you wish to use a chi-squared homogeneity test of two or more populations (not contingency between 1 population), expected() will return one list of lists of their expected values. For example, expected([[pop1 data], [pop2 data], [pop3 data]] returns
a flat list [pop1 expected data, pop2 expected data, pop3 expected data]. The following chi2_homogeneity() function uses this to calculate appropriate values in its hypothesis testing.

11) chi2_homogeneity(pop_lists)
Works in conjunction with previously defined function. Simply pass a list of lists and chi2_homogeneity will calculate an appropriate chi2 statistic and p-value to test for any difference in two or more populations for any given parameters. The data passed should be a list of lists that correlates to a 2-way table - each sublist is a "row" from the table, while each element in each sublist are the respective "columns." Automatically calculates an appropriate expected value and degree of freedom. This function heavily utilizes scipy.stat's chisquare() function, which is designed specifically for goodness of fit tests. chisquare() provides a good chi2 statistic but returns an invalid p-value for homogeneity, so this function automatically corrects this issue as well. Returns the chi2 statistic and the p-value. 

