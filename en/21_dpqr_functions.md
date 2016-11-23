# Probability distribution

Rcpp provides all major probability distribution functions in R. Same as R, four functions starting with the character d/p/q/r are defined for each probability distribution.

d/p/q/r functions on probability distribution XXX

* dXXX: Probability density function
* pXXX: Cumulative distribution function
* qXXX: Quantile function
* rXXX: Random number generation function


## Basic structure of probability distribution function

In Rcpp, probability distribution functions with the same name are defined in two namespaces, `R::` and `Rcpp::`. These differences are that the function defined in `Rcpp::` namespace returns a vector, while the function in the `R::` namespace returns a scalar. Basically, the probability distribution functions defined in the `Rcpp::` namespace has the same functionalities as those in R. So normally you can use a function in the `Rcpp::` namespace, but if you want a scalar value, it is better to use that function in `R::` namespace because it's faster.

The basic structures of the probability distribution functions defined in the `Rcpp::` namespace are shown below. In fact, the definition of the probability distribution function of the `Rcpp ::` namespace is not written directly in the source code of Rcpp (because it is written using macros). But you can assume that the function is defined like as below.

```cpp
NumericVector Rcpp::dXXX( NumericVector x, double par,                    bool log = false )
NumericVector Rcpp::pXXX( NumericVector q, double par, bool lower = true, bool log = false )
NumericVector Rcpp::qXXX( NumericVector p, double par, bool lower = true, bool log = false )
NumericVector Rcpp::rXXX(           int n, double par )
```

The basic structures of the probability distribution functions defined in the `R::` namespace are shown below.
It basically has the same functionality as those defined in the `Rcpp::` namespace except that it accepts and returns `double` type. In addition, the arguments of the function do not have default values, so user must give the value explicitly.

```cpp
double R::dXXX( double x, double par,            int log )
double R::pXXX( double q, double par, int lower, int log )
double R::qXXX( double p, double par, int lower, int log )
double R::rXXX(           double par )
```

The arguments of the probability distribution function are described below.

|Argument|Description|
|--|--|
|x, q|random variable|
|p|probability|
|n|number of observation|
|par|Parameter（the number of distribution parameters varies depending on the distribution）|
|lower|`true` : Calculate the probability of the region where the random variable is less than or equal to x, false : Calculate the probability of the region larger than x|
|log|true : probabilities p are given as log(p)|

## List of probability distribution functions

List of probability distribution functions provided by Rcpp is shown below. Here, the names of the distribution parameters are matched with those of R, so refer to the R help for details.


### Continuous probability distribution


- [Uniform distribution](#uniform-distribution)
- [Normal distribution](#normal-distribution)
- [Log-normal distribution](#log-normal-distribution)
- [Gamma distribution](#gamma-distribution)
- [Beta distribution](#beta-distribution)
- [Noncentral beta distribution](#noncentral-beta-distribution)
- [Chi-squared distribution](#chi-squared-distribution)
- [Noncentral chi-squared distribution](#noncentral-chi-squared-distribution)
- [t-distribution](#t-distribution)
- [Noncentral t-distribution](#noncentral-t-distribution)
- [F-distribution](#f-distribution)
- [Noncentral F-distribution](#noncentral-f-distribution)
- [Cauchy distribution](#cauchy-distribution)
- [Exponential distribution](#exponential-distribution)
- [Logistic distribution](#logistic-distribution)
- [Weibull distribution](#weibull-distribution)


### Discrete probability distribution

- [Binomial distribution](#二項分布)
- [Negative binomial distribution (Version with success probability as parameter)](#)
- [Negative binomial distribution (Version with mean as parameter)](#)
- [Poisson distribution](#poisson-distribution)
- [Geometric distribution](#geometric-distribution)
- [Hypergeometric distribution](#Hypergeometric distribution)
- [Distribution of Wilcoxon rank-sum test statistic](#distribution-of-wilcoxon-rank-sum-test-statistic)
- [Distribution of Wilcoxon signed-rank test statistic](#distribution-of-wilcoxon-signed-rank-test-statistic)


## Continuous probability distribution

### Uniform distribution

These functions provide information about the uniform distribution on the interval from `min` to `max`.

```cpp
Rcpp::dunif( x, min = 0.0, max = 1.0, log = false )
Rcpp::punif( x, min = 0.0, max = 1.0, lower = true, log = false )
Rcpp::qunif( q, min = 0.0, max = 1.0, lower = true, log = false )
Rcpp::runif( n, min = 0.0, max = 1.0 )

R::dunif( x, min, max,        log )
R::punif( x, min, max, lower, log )
R::qunif( q, min, max, lower, log )
R::runif(     min, max )
```


### Normal distribution

These functions provide information about the normal distribution with mean equal to `mean` and standard deviation equal to `sd`.

```cpp
Rcpp::dnorm( x, mean = 0.0, sd = 1.0, log = false )
Rcpp::pnorm( x, mean = 0.0, sd = 1.0, lower = true, log = false )
Rcpp::qnorm( q, mean = 0.0, sd = 1.0, lower = true, log = false )
Rcpp::rnorm( n, mean = 0.0, sd = 1.0 )

R::dnorm( x, mean, sd,        log )
R::pnorm( x, mean, sd, lower, log )
R::qnorm( q, mean, sd, lower, log )
R::rnorm(    mean, sd )
```

### Log-normal distribution

These functions provide information about the log-normal distribution whose logarithm has mean equal to `meanlog` and standard deviation equal to `sdlog`.

```cpp
Rcpp::dlnorm( x, meanlog = 0.0, sdlog = 1.0,               log = false )
Rcpp::plnorm( x, meanlog = 0.0, sdlog = 1.0, lower = true, log = false )
Rcpp::qlnorm( q, meanlog = 0.0, sdlog = 1.0, lower = true, log = false )
Rcpp::rlnorm( n, meanlog = 0.0, sdlog = 1.0 )

R::dlnorm( x, meanlog, sdlog,        log )
R::plnorm( x, meanlog, sdlog, lower, log )
R::qlnorm( q, meanlog, sdlog, lower, log )
R::rlnorm(    meanlog, sdlog )
```

### Gamma distribution

These functions provide information about the Gamma distribution with parameters `shape` and `scale`.

```cpp
Rcpp::dgamma( x, shape, scale = 1.0,               log = false )
Rcpp::pgamma( x, shape, scale = 1.0, lower = true, log = false )
Rcpp::qgamma( q, shape, scale = 1.0, lower = true, log = false )
Rcpp::rgamma( n, shape, scale = 1.0 )

R::dgamma( x, shape, scale, log )
R::pgamma( x, shape, scale, lower, log )
R::qgamma( q, shape, scale, lower, log )
R::rgamma(    shape, scale )
```

### Beta distribution

These functions provide information about the Beta distribution with parameters `shape1` and `shape2`. These functions are equivalent to setting 0 for the noncentrality parameter `ncp` in the Beta distribution function in R.

```cpp
Rcpp::dbeta( x, shape1, shape2, log = false )
Rcpp::pbeta( x, shape1, shape2, lower = true, log = false )
Rcpp::qbeta( q, shape1, shape2, lower = true, log = false )
Rcpp::rbeta( n, shape1, shape2)

R::dbeta( x, shape1, shape2,        log )
R::pbeta( x, shape1, shape2, lower, log )
R::qbeta( q, shape1, shape2, lower, log )
R::rbeta(    shape1, shape2 )
```

### Noncentral beta distribution

These functions provide information about the Beta distribution with parameters `shape1` and `shape2`. These functions are equivalent to setting non 0 value for the noncentrality parameter `ncp` in the Beta distribution function in R.

```cpp
Rcpp::dnbeta( x, shape1, shape2, ncp,               log = false );
Rcpp::pnbeta( x, shape1, shape2, ncp, lower = true, log = false );
Rcpp::qnbeta( q, shape1, shape2, ncp, lower = true, log = false );
// Rcpp::rnbeta() does not exist

R::dnbeta( x, shape1, shape2, ncp,        log )
R::pnbeta( x, shape1, shape2, ncp, lower, log )
R::qnbeta( q, shape1, shape2, ncp, lower, log )
R::rnbeta(    shape1, shape2, ncp )
```

### Chi-squared distribution

These functions provide information about the Chi-squared distribution with df degrees of freedom `df`. These functions are equivalent to setting 0 for the noncentrality parameter `ncp` in the Beta distribution function in R.

```cpp
Rcpp::dchisq( x, df,               log = false )
Rcpp::pchisq( x, df, lower = true, log = false )
Rcpp::qchisq( q, df, lower = true, log = false )
Rcpp::rchisq( n, df)

R::dchisq( x, df,        log )
R::pchisq( x, df, lower, log )
R::qchisq( q, df, lower, log )
R::rchisq(    df )
```




### Noncentral chi-squared distribution

These functions provide information about the Noncentral chi-squared distribution with df degrees of freedom `df`. These functions are equivalent to setting non 0 value for the noncentrality parameter `ncp` in the Chi-squared distribution function in R.

```cpp
Rcpp::dnchisq( x, df, ncp,               log = false )
Rcpp::pnchisq( x, df, ncp, lower = true, log = false )
Rcpp::qnchisq( q, df, ncp, lower = true, log = false )
Rcpp::rnchisq( n, df, ncp = 0.0 )

R::dnchisq( x, df, ncp,        log )
R::pnchisq( x, df, ncp, lower, log )
R::qnchisq( q, df, ncp, lower, log )
R::rnchisq(    df, ncp )
```

### t分布

自由度 df の t 分布の情報を与えます。これは R の t 分布関数において非心パラメータ ncp の値に 0 を設定した場合に相当します。

```cpp
Rcpp::dt( x, df,               log = false )
Rcpp::pt( x, df, lower = true, log = false )
Rcpp::qt( q, df, lower = true, log = false )
Rcpp::rt( n, df )

R::dt( x, df,        log )
R::pt( x, df, lower, log )
R::qt( q, df, lower, log )
R::rt(    df )
```

### 非心t分布

自由度 df、非心パラメータ ncp の t 分布の情報を与えます。ncp = 0 では t 分布に一致します。

```cpp
Rcpp::dnt( x, df, ncp,               log = false  )
Rcpp::pnt( x, df, ncp, lower = true, log = false  )
Rcpp::qnt( q, df, ncp, lower = true, log = false  )
// Rcpp::rnt関数は存在しません

R::dnt( x, df, ncp,        log )
R::pnt( x, df, ncp, lower, log )
R::qnt( q, df, ncp, lower, log )
// R::rnt関数は存在しません
```

### F分布

自由度 df1, df2 のF分布の情報を与えます。これは R のF分布関数において非心パラメータ ncp の値に 0 を設定した場合に相当します。

```cpp
Rcpp::df( x, df1, df2,               log = false )
Rcpp::pf( x, df1, df2, lower = true, log = false )
Rcpp::qf( q, df1, df2, lower = true, log = false )
Rcpp::rf( n, df1, df1 )

R::df( x, df1, df2,        log )
R::pf( x, df1, df2, lower, log )
R::qf( q, df1, df2, lower, log )
R::rf(    df1, df2 )
```


### 非心F分布

自由度 df1, df2 非心パラメータ ncp の F 分布の情報を与えます。ncp = 0 では F 分布に一致します。

```cpp
Rcpp::dnf( x, df1, df2, ncp,               log = false )
Rcpp::pnf( x, df1, df2, ncp, lower = true, log = false )
Rcpp::qnf( q, df1, df2, ncp, lower = true, log = false )
// Rcpp::rnf関数は存在しません

R::dnf( x, df1, df2, ncp,        log )
R::pnf( x, df1, df2, ncp, lower, log )
R::qnf( q, df1, df2, ncp, lower, log )
// R::rnf関数は存在しません
```

### コーシー分布

位置パラメータ location、尺度パラメータ scale のコーシー分布の情報を与えます。

```cpp
Rcpp::dcauchy( x, location = 0.0, scale = 1.0,               log = false )
Rcpp::pcauchy( x, location = 0.0, scale = 1.0, lower = true, log = false )
Rcpp::qcauchy( q, location = 0.0, scale = 1.0, lower = true, log = false )
Rcpp::rcauchy( n, location = 0.0, scale = 1.0)

R::dcauchy( x, location, scale,        log )
R::pcauchy( x, location, scale, lower, log )
R::qcauchy( q, location, scale, lower, log )
R::rcauchy(    location, scale )
```


### 指数分布

割合 rate (平均が1/rate) の指数分布の情報を与えます。

```cpp
Rcpp::dexp( x, rate = 1.0,               log = false )
Rcpp::pexp( x, rate = 1.0, lower = true, log = false )
Rcpp::qexp( q, rate = 1.0, lower = true, log = false )
Rcpp::rexp( n, rate = 1.0)

R::dexp( x, rate,        log )
R::pexp( x, rate, lower, log )
R::qexp( q, rate, lower, log )
R::rexp(    rate )
```

### ロジスティック分布

位置パラータ location 尺度パラメータ scale のロジスティック分布の情報を与えます。

```cpp
Rcpp::dlogis( x, location = 0.0, scale = 1.0,               log = false )
Rcpp::plogis( x, location = 0.0, scale = 1.0, lower = true, log = false )
Rcpp::qlogis( q, location = 0.0, scale = 1.0, lower = true, log = false )
Rcpp::rlogis( n, location = 0.0, scale = 1.0 )

R::dlogis( x, location, scale,        log )
R::plogis( x, location, scale, lower, log )
R::qlogis( q, location, scale, lower, log )
R::rlogis(    location, scale )
```


### ワイブル分布

形状パラメータ shape、尺度パラメータ scale のワイブル分布の情報を与えます。

```cpp
Rcpp::dweibull( x, shape, scale = 1.0,               log = false  )
Rcpp::pweibull( x, shape, scale = 1.0, lower = true, log = false  )
Rcpp::qweibull( q, shape, scale = 1.0, lower = true, log = false  )
Rcpp::rweibull( n, shape, scale = 1.0 )

R::dweibull( x, shape, scale,        log )
R::pweibull( x, shape, scale, lower, log )
R::qweibull( q, shape, scale, lower, log )
R::rweibull(    shape, scale )
```

## 離散分布



### 二項分布

試行回数 size 成功確率 prob の二項分布の情報を与えます。

```cpp
Rcpp::dbinom( x, size, prob,               log = false )
Rcpp::pbinom( x, size, prob, lower = true, log = false )
Rcpp::qbinom( q, size, prob, lower = true, log = false )
Rcpp::rbinom( n, size, prob )

R::dbinom( x, size, prob,        log )
R::pbinom( x, size, prob, lower, log )
R::qbinom( q, size, prob, lower, log )
R::rbinom(    size, prob )
```






### 負の二項分布（成功確率を指定するバージョン）

成功回数 size、１試行あたりの成功確率 prob の負の二項分布の情報を与えます。

```cpp
Rcpp::dnbinom( x, size, prob,               log = false )
Rcpp::pnbinom( x, size, prob, lower = true, log = false )
Rcpp::qnbinom( q, size, prob, lower = true, log = false )
Rcpp::rnbinom( n, size, prob )

R::dnbinom( x, size, prob,        log )
R::pnbinom( x, size, prob, lower, log )
R::qnbinom( q, size, prob, lower, log )
R::rnbinom(    size, prob )
```

### 負の二項分布（平均値を指定するバージョン）

成功回数 size、分布の平均が mu (=size/prob) の負の二項分布の情報を与えます。

```cpp
Rcpp::dnbinom_mu( x, size, mu,               log = false )
Rcpp::pnbinom_mu( x, size, mu, lower = true, log = false )
Rcpp::qnbinom_mu( q, size, mu, lower = true, log = false )
Rcpp::rnbinom_mu( n, size, mu )

R::dnbinom_mu( x, size, mu,        log )
R::pnbinom_mu( x, size, mu, lower, log )
R::qnbinom_mu( q, size, mu, lower, log )
R::rnbinom_mu(    size, mu )
```


### ポワソン分布

平均値と分散が lambda であるポワソン分布の情報を与えます。

```cpp
Rcpp::dpois( x, lambda,               log = false )
Rcpp::ppois( x, lambda, lower = true, log = false )
Rcpp::qpois( q, lambda, lower = true, log = false )
Rcpp::rpois( n, lambda )

R::dpois( x, lambda, log )
R::ppois( x, lambda, lower, log )
R::qpois( q, lambda, lower, log )
R::rpois(    lambda )
```







### 幾何分布

成功確率 prob の幾何分布の情報を与えます。

```cpp
Rcpp::dgeom( x, prob,               log = false )
Rcpp::pgeom( x, prob, lower = true, log = false )
Rcpp::qgeom( q, prob, lower = true, log = false )
Rcpp::rgeom( n, prob )

R::dgeom( x, prob, log )
R::pgeom( x, prob, lower, log )
R::qgeom( q, prob, lower, log )
R::rgeom(    prob )
```

### 超幾何分布

母集団に含まれる成功数 m、母集団に含まれる失敗数 n、母集団からサンプリングする標本の数 k の超幾何分布の情報を与えます。

```cpp
Rcpp::dhyper( x, m, n, k,               log = false )
Rcpp::phyper( x, m, n, k, lower = true, log = false )
Rcpp::qhyper( q, m, n, k, lower = true, log = false )
Rcpp::rhyper(nn, m, n, k )

R::dhyper( x, m, n, k,        log )
R::phyper( x, m, n, k, lower, log )
R::qhyper( q, m, n, k, lower, log )
R::rhyper(    m, n, k )
```

### ウィルコクソン順位和検定統計量の分布

標本数がそれぞれ m、n である２つの標本に対してウィルコクソン順位和検定（マン・ホイットニーのU検定）を行ったときの検定統計量の分布の情報を与えます。

```cpp
// Rcpp::dwilcox関数は存在しません
// Rcpp::pwilcox関数は存在しません
// Rcpp::qwilcox関数は存在しません
Rcpp::rwilcox( nn, m, n );

R::dwilcox( x, m, n,        log )
R::pwilcox( x, m, n, lower, log )
R::qwilcox( q, m, n, lower, log )
R::rwilcox(    m, n )
```



### ウィルコクソン符号順位検定統計量の分布

n 個の標本への各2回の観察に対してウィルコクソン符号順位検定を行ったときの検定統計量の分布の情報を与えます。


```cpp
// Rcpp::dsignrank関数は存在しません
// Rcpp::psignrank関数は存在しません
// Rcpp::qsignrank関数は存在しません
Rcpp::rsignrank( nn, n )

R::dsignrank( x, n,        log )
R::psignrank( x, n, lower, log )
R::qsignrank( q, n, lower, log )
R::rsignrank(    n )
```
