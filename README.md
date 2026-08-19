# Exotic Oprions Pricer - GBM Monte Carlo Simulation

In this case, option payoffs depend not only on the stock price at expiration but also upon the history of the stock price sampled at various points during the life of the option. Assume that the stock price will be needed at the end of each of the next ***12*** months, in addition to $S_0$ today, and start by simulating ***10,000*** such stock price paths. In particular:

### Prep: 
* Generate random variables that follow a standard normal distribution. This will require a table with 10,000 rows and 12 columns. Denote the value of the random variable on path $i$ for month $j$ by $Z_{i,j}$ where $i = 1, . . . ,10,000$ and $j = M_1, . . . , M_{12}$
* Generate a table with 10,000 stock price paths $S_{i,j}$ where $i = 1, . . . ,10,000$ and $j = M_0, . . . , M_{12}$. In each case, $S_{i,0} = S_0$. For the remaining entries, define $\Delta t = \frac{1}{12}$ and set $S_{i,j} = S_{i,j−1} \cdot e^{(r - \frac{\sigma^2}{2})\Delta t + \sigma \sqrt{\Delta t} z_{i,j}}$ for $i = 1, . . . ,10,000$ and $j = 1, . . . ,12$. We use risk-free rate $r$ here instead of the expected rate of return on the underlying stock because of risk-neutral valuation. Note that $S_T$ on each simulated path $i$ is defined to be equal to $S_{i,12}$.
* Construct another table with 10,000 rows and 6 columns storing discounted payoffs (to the present values) at expiration for various different options, as indicated below:
    * European call option, with payoff max($S_T - K$, 0).
    * European put option, with payoff max($K - S_T$, 0).
    * Average price put option, with payoff max($K -  \bar S$, 0), where $\bar S = \frac{S_0 + S_1 + \cdots + S_{12}}{13}$.
    * Floating lookback call option, with payoff $S_T - S_{min}$, where $S_{min}$ on each path is the minimum of the stock prices $S_0, S_1, \cdots, S_{12}$ on that path.
    * Up-and-out call option with barrier B = $90, with payoff max($S_T - K$, 0) as long as none of the observed monthly stock prices on the given path are greater than or equal to B. If any of the monthly stock prices are greater than or equal to B, then the payoff is zero.
    * Up-and-in call option with barrier B = $90, with payoff max($S_T - K$, 0) as long as at least one of the observed monthly stock prices on the given path is greater than or equal to B. If this condition is not satisfied, the option payoff is zero.
* Create another table which shows the following quantities for each of the options listed above (below N is the number of simulated paths, which is 10,000 here):
    * Mean discounted payoff $\mu$. This is the estimated option premium.
    * Standard error of option premium $\hat \sigma$. This is the standard deviation of the option payoff divided by $\sqrt N$ .
    * Lower bound for 95% confidence interval for option premium, which is $\mu - 1.96 \hat \sigma$.
    * Upper bound for 95% confidence interval for option premium, which is $\mu + 1.96 \hat \sigma$.
 
### Improvement:
> One disadvantage of Monte Carlo methods is that they can be slow to converge. On the other hand, there are situations where they are the only methods that can be used. As a result, ***antithetic variates*** is introduced to reduce the vairance and improve the rate of convergence.  This relies on the fact that if Z is a standard normal random variable, then so is −Z.

* Generate another 10,000 stock price paths, but this time using $−Z_{i,j}$ instead of $Z_{i,j}$ throughout.
* Provide another table of variates discounted payoffs, report the average discounted payoff along each path $i$ from the cases which have $Z_{i,j}$ and the cases which have $−Z_{i,j}$. 
* Make a final table similar to the last one from part *Prep*, containing the mean average discounted payoff, its standard error, and the 95% confidence interval bounds. Note that when calculating the standard error, continue to use **N = 10,000**. This is because we still have only 10,000 independent paths, even though there are 20,000 paths in total.
