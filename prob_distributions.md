# d/p/q/r 関数

Rcpp は R にある主要な全ての d/p/q/r 関数を提供する。

* d(x): density function
* p(q): cumulative distribution function
* q(p): quantile function
* r(n): random generation


###Rcpp::d/p/q/r関数の基本構造

```
NumericVector Rcpp::dXXX( NumericVector x, double p0, bool log = false);
NumericVector Rcpp::pXXX( NumericVector q, double p0, bool lower = true, bool log = false);
NumericVector Rcpp::qXXX( NumericVector p, double p0, bool lower = true, bool log = false);
NumericVector Rcpp::rXXX(           int n, double p0);
```
上は、d/p/q/r関数の基本構造を概念的に表したものなので、Rcppのソースコード中の記述とは正確には異なっている。通常のユーザーにとってはこのような関数が定義されていると考えても差し支えない。

そもそも `Rcpp::d/p/q/r` 関数はマクロで記述されているので、ソースコード中に明示的には `Rcpp::d/p/q/r` の定義は書いていない。

###R版との違い

`Rcpp::` 名前空間で定義されている d/p/q/r 関数は、基本的にRの関数と同じ機能を持っているが、違いもある。具体的には、Rでは分布パラメータ引数（上で p0 で示されている）のデフォルト値が自動的に与えられる場合でも、Rcppでは与えられていない。そのためユーザーが分布パラメータの値を明示的に与えなければならない。



###R::とRcpp::

一般的に `Rcpp::` 名前空間で定義されている d/p/q/r の第１引数はベクトル化されている。同じ名前の関数が `R::`名前空間の中で定義されているが、こちらはベクトル化されていないので、注意すること。

`R::` 名前空間で定義されている d/p/q/r 関数の基本構造

```
double R::dXXX(double x, double p0, int lg)			
double R::pXXX(double x, double p0, int lt, int lg)		
double R::qXXX(double p, double p0, int lt, int lg)		
double R::rXXX(double p0)
```

## d/p/q/r関数

以下ではRcppが提供するd/p/q/r関数の形式を模式的に示す。

beta, binom, cauchy, chisq, exp, f, gamma, geom, hyper, lnorm, logis, nbeta, nbinom_mu, nbinom, nchisq, nf, norm, nt, pois, t, unif, weibull


###beta

```cpp
NumericVector Rcpp::dbeta(NumericVector x, double a, double b, bool log = false);
NumericVector Rcpp::pbeta(NumericVector q, double p, double q, bool lower = true, bool log = false);
NumericVector Rcpp::qbeta(NumericVector p, double p, double q, bool lower = true, bool log = false);
NumericVector Rcpp::rbeta( int n, double a, double b);
```

```cpp
double R::dbeta(double x, double a, double b, int lg)         
double R::pbeta(double x, double p, double q, int lt, int lg) 
double R::qbeta(double a, double p, double q, int lt, int lg) 
double R::rbeta(double a, double b) 
```



###binom

```cpp
NumericVector Rcpp::dbinom(NumericVector x, double n, double p, bool log = false);
NumericVector Rcpp::pbinom(NumericVector x, double n, double p, bool lower = true, bool log = false);
NumericVector Rcpp::qbinom(NumericVector p, double n, double m, bool lower = true, bool log = false);
NumericVector Rcpp::rbinom( int n, double nin, double pp )
```

```cpp
double R::dbinom(double x, double n, double p, int lg)	  	
double R::pbinom(double x, double n, double p, int lt, int lg)  
double R::qbinom(double p, double n, double m, int lt, int lg)  
double R::rbinom(double n, double p)
```



###cauchy

```
NumericVector Rcpp::dcauchy(NumericVector x, double lc, double sl, bool log = false)
NumericVector Rcpp::pcauchy(NumericVector x, double lc, double sl, bool lower = true, bool log = false)
NumericVector Rcpp::qcauchy(NumericVector p, double lc, double sl, bool lower = true, bool log = false)
NumericVector rcauchy( int n, double location, double scale )
NumericVector rcauchy( int n, double location /* , double scale [=1.0] */ )
NumericVector rcauchy( int n /*, double location [=0.0] , double scale [=1.0] */ )
```

```
double R::dcauchy(double x, double lc, double sl, int lg)		
double R::pcauchy(double x, double lc, double sl, int lt, int lg)	
double R::qcauchy(double p, double lc, double sl, int lt, int lg)	
double R::rcauchy(double lc, double sl)
```




###chisq

```
NumericVector dchisq(NumericVector x, double df, bool log = false)
NumericVector pchisq(NumericVector x, double df, bool lower = true, bool log = false)
NumericVector qchisq(NumericVector p, double df, bool lower = true, bool log = false)
NumericVector rchisq(int n, double df)    
```


```
double dchisq(double x, double df, int lg)          
double pchisq(double x, double df, int lt, int lg)  
double qchisq(double p, double df, int lt, int lg)  
double rchisq(double df)    
```



###exp

Rcpp::

```
NumericVector dexp(double x, double sl, bool log = false)
NumericVector pexp(double x, double sl, bool lower = true, bool log = false)
NumericVector qexp(double p, double sl, bool lower = true, bool log = false)
NumericVector rexp( int n, double rate )
NumericVector rexp( int n /* , rate = 1 */ )
```

R::
```
double dexp(double x, double sl, int lg)		
double pexp(double x, double sl, int lt, int lg)	
double qexp(double p, double sl, int lt, int lg)	
double rexp(double sl)	

```



###f

Rcpp::
```
NumericVector df(NumericVector x, double df1, double df2, bool log = false)
NumericVector pf(NumericVector x, double df1, double df2, bool lower = true, bool log = false)
NumericVector qf(NumericVector p, double df1, double df2, bool lower = true, bool log = false)
NumericVector rf( int n, double n1, double n2 )
```
R::
```
double df(double x, double df1, double df2, int lg)		
double pf(double x, double df1, double df2, int lt, int lg)	
double qf(double p, double df1, double df2, int lt, int lg)	
double rf(double df1, double df2)
```

###gamma

```
NumericVector dgamma(NumericVector x, double shp, double scl, bool log = false)
NumericVector pgamma(NumericVector x, double alp, double scl, bool log = false)
NumericVector qgamma(NumericVector p, double alp, double scl, bool log = false)
NumericVector rgamma( int n, double a, double scale )
NumericVector rgamma( int n, double a /* scale = 1.0 */ )
```

```
double dgamma(double x, double shp, double scl, int lg)	   
double pgamma(double x, double alp, double scl, int lt, int lg) 
double qgamma(double p, double alp, double scl, int lt, int lg) 
double rgamma(double a, double scl)  
```

###geom
NumericVector rgeom( int n, double p )

```
double dgeom(double x, double p, int lg)		
double pgeom(double x, double p, int lt, int lg)	
double qgeom(double p, double pb, int lt, int lg)	
double rgeom(double p)
```

###hyper
NumericVector rhyper( int n, double nn1, double nn2, double kk )

```
double dhyper(double x, double r, double b, double n, int lg)		
double phyper(double x, double r, double b, double n, int lt, int lg)	
double qhyper(double p, double r, double b, double n, int lt, int lg)	
double rhyper(double r, double b, double n)					

```
###lnorm

NumericVector rlnorm( int n, double meanlog, double sdlog )
NumericVector rlnorm( int n, double meanlog /*, double sdlog = 1.0 */)
NumericVector rlnorm( int n /*, double meanlog [=0.], double sdlog = 1.0 */)

```
double dlnorm(double x, double ml, double sl, int lg)	 
double plnorm(double x, double ml, double sl, int lt, int lg) 
double qlnorm(double p, double ml, double sl, int lt, int lg) 
double rlnorm(double ml, double sl)    
```

###logis

NumericVector rlogis( int n, double location, double scale )
NumericVector rlogis( int n, double location /*, double scale =1.0 */ )
NumericVector rlogis( int n /*, double location [=0.0], double scale =1.0 */ )

double dlogis(double x, double lc, double sl, int lg)		
double plogis(double x, double lc, double sl, int lt, int lg)	
double qlogis(double p, double lc, double sl, int lt, int lg)	
double rlogis(double lc, double sl)			

###nbeta
```cpp
NumericVector Rcpp::dnbeta(double x, double a, double b, double ncp, , bool log = false);
NumericVector Rcpp::pnbeta(double x, double a, double b, double ncp,  bool lower = true, bool log = false);
NumericVector Rcpp::qnbeta(double p, double a, double b, double ncp, bool lower = true, bool log = false);
//NumericVector Rcpp::rnbeta( int n, double a, double b, double np) //not defined
```
```cpp
double R::dnbeta(double x, double a, double b, double ncp, int lg)		
double R::pnbeta(double x, double a, double b, double ncp, int lt, int lg)	
double R::qnbeta(double p, double a, double b, double ncp, int lt, int lg)	
double R::rnbeta(double a, double b, double np)
```



###nbinom_mu
double dnbinom_mu(double x, double sz, double mu, int lg)		
double pnbinom_mu(double x, double sz, double mu, int lt, int lg)	
double qnbinom_mu(double x, double sz, double mu, int lt, int lg)	
double rnbinom_mu(double sz, double mu)

###nbinom

NumericVector rnbinom( int n, double siz, double prob )
NumericVector rnbinom_mu( int n, double siz, double mu )

double dnbinom(double x, double sz, double pb, int lg)		
double pnbinom(double x, double sz, double pb, int lt, int lg)	
double qnbinom(double p, double sz, double pb, int lt, int lg)	
double rnbinom(double sz, double pb)

###nchisq
NumericVector rnchisq( int n, double df, double lambda )
NumericVector rnchisq( int n, double df /*, double lambda = 0.0 */ )
```
double dnchisq(double x, double df, double ncp, int lg)         
double pnchisq(double x, double df, double ncp, int lt, int lg) 
double qnchisq(double p, double df, double ncp, int lt, int lg) 
double rnchisq(double df, double lb)   
```

###nf
double dnf(double x, double df1, double df2, double ncp, int lg)		
double pnf(double x, double df1, double df2, double ncp, int lt, int lg)	
double qnf(double p, double df1, double df2, double ncp, int lt, int lg)

###norm


NumericVector rnorm( int n, double mean, double sd)
NumericVector rnorm( int n, double mean /*, double sd [=1.0] */ )
NumericVector rnorm( int n /*, double mean [=0.0], double sd [=1.0] */ )

```
double dnorm(double x, double mu, double sigma, int lg)
double pnorm(double x, double mu, double sigma, int lt, int lg)
double qnorm(double p, double mu, double sigma, int lt, int lg)
double rnorm(double mu, double sigma)
```




###nt
double dnt(double x, double df, double ncp, int lg)		
double pnt(double x, double df, double ncp, int lt, int lg)	
double qnt(double p, double df, double ncp, int lt, int lg)	

###pois
NumericVector rpois( int n, double mu )

double dpois(double x, double lb, int lg)		
double ppois(double x, double lb, int lt, int lg)	
double qpois(double p, double lb, int lt, int lg)	
double rpois(double mu)	

###t

NumericVector rt( int n, double df )

double dt(double x, double n, int lg)			
double pt(double x, double n, int lt, int lg)		
double qt(double p, double n, int lt, int lg)		
double rt(double n)		

###unif

NumericVector runif( int n, double min, double max )
NumericVector runif( int n, double min /*, double max = 1.0 */ )
NumericVector runif( int n /*, double min = 0.0, double max = 1.0 */ )

```
double dunif(double x, double a, double b, int lg)
double punif(double x, double a, double b, int lt, int lg)
double qunif(double p, double a, double b, int lt, int lg)
double runif(double a, double b)
```
###weibull

NumericVector rweibull( int n, double shape, double scale )
NumericVector rweibull( int n, double shape /* scale = 1 */ )

double dweibull(double x, double sh, double sl, int lg)		
double pweibull(double x, double sh, double sl, int lt, int lg)	
double qweibull(double p, double sh, double sl, int lt, int lg)	
double rweibull(double sh, double sl)

分布乱数

```
NumericVector rwilcox( int n, double mm, double nn )
NumericVector rsignrank( int n, double nn )
```















```