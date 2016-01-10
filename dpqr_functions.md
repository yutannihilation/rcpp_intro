# d/p/q/r 関数

Rcpp は R にある主要な全ての d/p/q/r 関数を提供する。

* d: density function
* p: cumulative distribution function
* q: quantile function
* r: random generation


###Rcpp::d/p/q/r関数の基本構造

```cpp
NumericVector Rcpp::dXXX( NumericVector x, double p0, bool log = false)
NumericVector Rcpp::pXXX( NumericVector q, double p0, bool lower = true, bool log = false)
NumericVector Rcpp::qXXX( NumericVector p, double p0, bool lower = true, bool log = false)
NumericVector Rcpp::rXXX(           int n, double p0)
```
上は、d/p/q/r関数の基本構造を概念的に表したものなので、Rcppのソースコード中の記述とは正確には異なっている。通常のユーザーにとってはこのような関数が定義されていると考えても差し支えはない。

そもそも `Rcpp::d/p/q/r` 関数はマクロで記述されているので、ソースコード中に明示的には `Rcpp::d/p/q/r` の定義は書いていない。

###R版との違い

`Rcpp::` 名前空間で定義されている d/p/q/r 関数は、基本的にRの関数と同じ機能を持っているが、違いもある。具体的には、Rcppでは分布パラメータ引数（上の基本構造では p0 で示されている）のデフォルト値が与えられていない。そのためユーザーが値を明示的に与える。



###R::とRcpp::

一般的に `Rcpp::` 名前空間で定義されている d/p/q/r の第１引数はベクトル化されている。同じ名前の関数が `R::`名前空間の中で定義されているが、こちらはベクトル化されていないので、注意すること。

`R::` 名前空間で定義されている d/p/q/r 関数の基本構造

```cpp
double R::dXXX(double x, double p0, int lg)			
double R::pXXX(double x, double p0, int lt, int lg)		
double R::qXXX(double p, double p0, int lt, int lg)		
double R::rXXX(double p0)
```

## d/p/q/r関数

以下ではRcppが提供するd/p/q/r関数の形式を模式的に示す。

beta, binom, cauchy, chisq, exp, f, gamma, geom, hyper, lnorm, logis, nbeta, nbinom_mu, nbinom, nchisq, nf, norm, nt, pois, t, unif, weibull


###beta

Rcpp::


```cpp
NumericVector dbeta(NumericVector x, double a, double b, bool log = false)
NumericVector pbeta(NumericVector q, double p, double q, bool lower = true, bool log = false)
NumericVector qbeta(NumericVector p, double p, double q, bool lower = true, bool log = false)
NumericVector rbeta( int n, double a, double b);
```

R::

```cpp
double dbeta(double x, double a, double b, int lg)         
double pbeta(double x, double p, double q, int lt, int lg) 
double qbeta(double a, double p, double q, int lt, int lg) 
double rbeta(double a, double b) 
```



###binom

Rcpp::
```cpp
NumericVector dbinom(NumericVector x, double n, double p, bool log = false)
NumericVector pbinom(NumericVector x, double n, double p, bool lower = true, bool log = false)
NumericVector qbinom(NumericVector p, double n, double m, bool lower = true, bool log = false)
NumericVector rbinom( int n, double nin, double pp )
```
R::
```cpp
double dbinom(double x, double n, double p, int lg)	  	
double pbinom(double x, double n, double p, int lt, int lg)  
double qbinom(double p, double n, double m, int lt, int lg)  
double rbinom(double n, double p)
```



###cauchy

Rcpp::
```cpp
NumericVector dcauchy(NumericVector x, double lc = 0.0, double sl = 1.0, bool log = false)
NumericVector pcauchy(NumericVector x, double lc = 0.0, double sl = 1.0, bool lower = true, bool log = false)
NumericVector qcauchy(NumericVector p, double lc = 0.0, double sl = 1.0, bool lower = true, bool log = false)
NumericVector rcauchy( int n, double location = 0.0, double scale = 1.0)
```
R::
```cpp
double dcauchy(double x, double lc, double sl, int lg)		
double pcauchy(double x, double lc, double sl, int lt, int lg)	
double qcauchy(double p, double lc, double sl, int lt, int lg)	
double rcauchy(double lc, double sl)
```




###chisq
Rcpp::
```cpp
NumericVector dchisq(NumericVector x, double df, bool log = false)
NumericVector pchisq(NumericVector x, double df, bool lower = true, bool log = false)
NumericVector qchisq(NumericVector p, double df, bool lower = true, bool log = false)
NumericVector rchisq(int n, double df)    
```

R::
```cpp
double dchisq(double x, double df, int lg)          
double pchisq(double x, double df, int lt, int lg)  
double qchisq(double p, double df, int lt, int lg)  
double rchisq(double df)    
```



###exp

Rcpp::

```cpp
NumericVector dexp(double x, double rate = 1.0, bool log = false)
NumericVector pexp(double x, double rate = 1.0, bool lower = true, bool log = false)
NumericVector qexp(double p, double rate = 1.0, bool lower = true, bool log = false)
NumericVector rexp(   int n, double rate = 1.0)
```

R::
```cpp
double dexp(double x, double sl, int lg)		
double pexp(double x, double sl, int lt, int lg)	
double qexp(double p, double sl, int lt, int lg)	
double rexp(double sl)	

```



###f

Rcpp::
```cpp
NumericVector df(NumericVector x, double df1, double df2, bool log = false)
NumericVector pf(NumericVector x, double df1, double df2, bool lower = true, bool log = false)
NumericVector qf(NumericVector p, double df1, double df2, bool lower = true, bool log = false)
NumericVector rf( int n, double n1, double n2 )
```
R::
```cpp
double df(double x, double df1, double df2, int lg)		
double pf(double x, double df1, double df2, int lt, int lg)	
double qf(double p, double df1, double df2, int lt, int lg)	
double rf(double df1, double df2)
```

###gamma

Rcpp::
```cpp
NumericVector dgamma(NumericVector x, double shp, double scl = 1.0, bool log = false)
NumericVector pgamma(NumericVector x, double alp, double scl = 1.0, bool lower = true, bool log = false)
NumericVector qgamma(NumericVector p, double alp, double scl = 1.0, bool lower = true, bool log = false)
NumericVector rgamma( int n, double a, double scale = 1.0)
```
R::
```cpp
double dgamma(double x, double shp, double scl, int lg)	   
double pgamma(double x, double alp, double scl, int lt, int lg) 
double qgamma(double p, double alp, double scl, int lt, int lg) 
double rgamma(double a, double scl)  
```

###geom
Rcpp::
```cpp
NumericVector dgeom(NumericVector x, double p, bool log = false)
NumericVector pgeom(NumericVector x, double p, bool lower = true, bool log = false)
NumericVector qgeom(NumericVector p, double pb, bool lower = true, bool log = false)
NumericVector rgeom( int n, double p )
```
R::
```cpp
double dgeom(double x, double p, int lg)		
double pgeom(double x, double p, int lt, int lg)	
double qgeom(double p, double pb, int lt, int lg)	
double rgeom(double p)
```

###hyper
Rcpp::
```cpp
NumericVector dhyper(NumericVector x, double r, double b, double n, bool log = false)
NumericVector phyper(NumericVector x, double r, double b, double n, bool lower = true, bool log = false)
NumericVector qhyper(NumericVector p, double r, double b, double n, bool lower = true, bool log = false)
NumericVector rhyper( int n, double nn1, double nn2, double kk )
```
R::
```cpp
double dhyper(double x, double r, double b, double n, int lg)		
double phyper(double x, double r, double b, double n, int lt, int lg)	
double qhyper(double p, double r, double b, double n, int lt, int lg)	
double rhyper(double r, double b, double n)					

```
###lnorm


Rcpp::
```cpp
NumericVector dlnorm(NumericVector x, double ml = 0.0, double sl = 1.0, bool log = false)
NumericVector plnorm(NumericVector x, double ml = 0.0, double sl = 1.0, bool lower = true, bool log = false)
NumericVector qlnorm(NumericVector p, double ml = 0.0, double sl = 1.0, bool lower = true, bool log = false)
NumericVector rlnorm( int n, double meanlog = 0.0, double sdlog = 1.0)
```
R::
```cpp
double dlnorm(double x, double ml, double sl, int lg)	 
double plnorm(double x, double ml, double sl, int lt, int lg) 
double qlnorm(double p, double ml, double sl, int lt, int lg) 
double rlnorm(double ml, double sl)    
```

###logis
Rcpp::
```cpp
NumericVector dlogis(NumericVector x, double lc = 0.0, double sl = 1.0, bool log = false)
NumericVector plogis(NumericVector x, double lc = 0.0, double sl = 1.0, bool lower = true, bool log = false)
NumericVector qlogis(NumericVector p, double lc = 0.0, double sl = 1.0, bool lower = true, bool log = false)
NumericVector rlogis( int n, double location, double scale )
NumericVector rlogis( int n, double location /*, double scale =1.0 */ )
NumericVector rlogis( int n /*, double location [=0.0], double scale =1.0 */ )
```
R::
```cpp
double dlogis(double x, double lc, double sl, int lg)		
double plogis(double x, double lc, double sl, int lt, int lg)	
double qlogis(double p, double lc, double sl, int lt, int lg)	
double rlogis(double lc, double sl)			
```

###nbeta
Rcpp::
```cpp
NumericVector dnbeta(double x, double a, double b, double ncp, , bool log = false);
NumericVector pnbeta(double x, double a, double b, double ncp,  bool lower = true, bool log = false);
NumericVector qnbeta(double p, double a, double b, double ncp, bool lower = true, bool log = false);
//NumericVector rnbeta( int n, double a, double b, double np) //not defined
```
R::
```cpp
double dnbeta(double x, double a, double b, double ncp, int lg)		
double pnbeta(double x, double a, double b, double ncp, int lt, int lg)	
double qnbeta(double p, double a, double b, double ncp, int lt, int lg)	
double rnbeta(double a, double b, double np)
```



###nbinom_mu
Rcpp::
```cpp
NumericVector dnbinom_mu(NumericVector x, double sz, double mu, bool log = false)
NumericVector pnbinom_mu(NumericVector x, double sz, double mu, bool lower = true, bool log = false)
NumericVector qnbinom_mu(NumericVector x, double sz, double mu, bool lower = true, bool log = false)
NumericVector rnbinom_mu( int n, double siz, double mu )
```
R::
```cpp
double dnbinom_mu(double x, double sz, double mu, int lg)		
double pnbinom_mu(double x, double sz, double mu, int lt, int lg)	
double qnbinom_mu(double x, double sz, double mu, int lt, int lg)	
double rnbinom_mu(double sz, double mu)
```

###nbinom
Rcpp::
```cpp
NumericVector dnbinom(NumericVector x, double sz, double pb, bool log = false)
NumericVector pnbinom(NumericVector x, double sz, double pb, bool lower = true, bool log = false)
NumericVector qnbinom(NumericVector p, double sz, double pb, bool lower = true, bool log = false)
NumericVector rnbinom( int n, double siz, double prob )
NumericVector rnbinom_mu( int n, double siz, double mu )
```
R::
```cpp
double dnbinom(double x, double sz, double pb, int lg)		
double pnbinom(double x, double sz, double pb, int lt, int lg)	
double qnbinom(double p, double sz, double pb, int lt, int lg)	
double rnbinom(double sz, double pb)
```

###nchisq
Rcpp::
```cpp
NumericVector dnchisq(NumericVector x, double df, double ncp, bool log = false)
NumericVector pnchisq(NumericVector x, double df, double ncp, bool lower = true, bool log = false)
NumericVector qnchisq(NumericVector p, double df, double ncp, bool lower = true, bool log = false)
NumericVector rnchisq( int n, double df, double lambda )
NumericVector rnchisq( int n, double df /*, double lambda = 0.0 */ )
```
R::
```cpp
double dnchisq(double x, double df, double ncp, int lg)         
double pnchisq(double x, double df, double ncp, int lt, int lg) 
double qnchisq(double p, double df, double ncp, int lt, int lg) 
double rnchisq(double df, double lb)   
```

###nf
Rcpp::

```cpp
NumericVector dnf(NumericVector x, double df1, double df2, double ncp, bool log = false)
NumericVector pnf(NumericVector x, double df1, double df2, double ncp, bool lower = true, bool log = false)
NumericVector qnf(NumericVector p, double df1, double df2, double ncp, bool lower = true, bool log = false)
```
R::
```cpp
double dnf(double x, double df1, double df2, double ncp, int lg)		
double pnf(double x, double df1, double df2, double ncp, int lt, int lg)	
double qnf(double p, double df1, double df2, double ncp, int lt, int lg)
```


###norm
Rcpp::
```cpp
NumericVector dnorm(NumericVector x, double mu = 0.0, double sigma = 1.0, bool log = false)
NumericVector pnorm(NumericVector x, double mu = 0.0, double sigma = 1.0, bool lower = true, bool log = false)
NumericVector qnorm(NumericVector p, double mu = 0.0, double sigma = 1.0, bool lower = true, bool log = false)
NumericVector rnorm( int n, double mean 0.0, double sd = 1.0)
```
R::
```cpp
double dnorm(double x, double mu, double sigma, int lg)
double pnorm(double x, double mu, double sigma, int lt, int lg)
double qnorm(double p, double mu, double sigma, int lt, int lg)
double rnorm(double mu, double sigma)
```




###nt
Rcpp::

```cpp
NumericVector dnt(NumericVector x, double df, double ncp, int lg)		
NumericVector pnt(NumericVector x, double df, double ncp, int lt, int lg)	
NumericVector qnt(NumericVector p, double df, double ncp, int lt, int lg)
```
R::
```cpp
double dnt(double x, double df, double ncp, int lg)		
double pnt(double x, double df, double ncp, int lt, int lg)	
double qnt(double p, double df, double ncp, int lt, int lg)	
```

###pois
Rcpp::
```cpp
NumericVector dpois(NumericVector x, double lb, bool log = false)
NumericVector ppois(NumericVector x, double lb, bool lower = true, bool log = false)
NumericVector qpois(NumericVector p, double lb, bool lower = true, bool log = false)
NumericVector rpois( int n, double mu )
```
R::
```cpp
double dpois(double x, double lb, int lg)		
double ppois(double x, double lb, int lt, int lg)	
double qpois(double p, double lb, int lt, int lg)	
double rpois(double mu)	
```

###t
Rcpp::
```cpp
NumericVector dt(NumericVector x, double n, bool log = false)
NumericVector pt(NumericVector x, double n, bool lower = true, bool log = false)
NumericVector qt(NumericVector p, double n, bool lower = true, bool log = false)
NumericVector rt( int n, double df )
```
R::
```cpp
double dt(double x, double n, int lg)			
double pt(double x, double n, int lt, int lg)		
double qt(double p, double n, int lt, int lg)		
double rt(double n)		
```

###unif
Rcpp::
```cpp
NumericVector dunif(NumericVector x, double a = 0.0, double b = 1.0, bool log = false)
NumericVector punif(NumericVector x, double a = 0.0, double b = 1.0, bool lower = true, bool log = false)
NumericVector qunif(NumericVector p, double a = 0.0, double b = 1.0, bool lower = true, bool log = false)
NumericVector runif( int n, double min = 0.0, double max = 1.0)
```
R::
```cpp
double dunif(double x, double a, double b, int lg)
double punif(double x, double a, double b, int lt, int lg)
double qunif(double p, double a, double b, int lt, int lg)
double runif(double a, double b)
```


###weibull
Rcpp::
```cpp
NumericVector dweibull(NumericVector x, double sh, double sl = 1.0, int lg)	
NumericVector pweibull(NumericVector x, double sh, double sl = 1.0, int lt, int lg)	
NumericVector qweibull(NumericVector p, double sh, double sl = 1.0, int lt, int lg)	
NumericVector rweibull( int n, double shape, double scale  = 1.0)
```
R::
```cpp
double dweibull(double x, double sh, double sl, int lg)		
double pweibull(double x, double sh, double sl, int lt, int lg)	
double qweibull(double p, double sh, double sl, int lt, int lg)	
double rweibull(double sh, double sl)
```




NumericVector rsignrank( int n, double nn )
NumericVector rwilcox( int n, double mm, double nn )

