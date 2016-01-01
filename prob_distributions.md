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

`Rcpp::` 名前空間で定義されている関数は、基本的にRの関数と同じ機能を持っているが、違いもある。具体的には、Rでは分布パラメータ引数（上で p0 で示されている）のデフォルト値が自動的に与えられる場合でも、Rcppでは与えられていない、そのためユーザーが値を明示的に与えなければならない。



###R::とRcpp::

一般的に `Rcpp::` 名前空間で定義されている d/p/q/r の第１引数はベクトル化されている。同じ名前の関数が `R::`名前空間の中で定義されているが、こちらはベクトル化されていないので、注意すること。 (Rmath.h)

```
double R::dXXX(double x, double p0, double p1, int lg);
```

## d/p/q/r関数

以下ではRcppが提供するd/p/q/r関数の形式を模式的に示す。


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

###Noncentral beta

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

###binom


```cpp
NumericVector Rcpp::dbinom(NumericVector x, double n, double p, bool log = false);
NumericVector Rcpp::pbinom(NumericVector x, double n, double p, bool lower = true, bool log = false);
NumericVector Rcpp::qbinom(NumericVector p, double n, double m, bool lower = true, bool log = false);
NumericVector Rcpp::rbinom(int n, double n, double p)
```

```cpp
double R::dbinom(double x, double n, double p, int lg)	  	
double R::pbinom(double x, double n, double p, int lt, int lg)  
double R::qbinom(double p, double n, double m, int lt, int lg)  
double R::rbinom(double n, double p)
```


分布乱数

```
NumericVector rnorm( int n, double mean, double sd)
NumericVector rnorm( int n, double mean /*, double sd [=1.0] */ )
NumericVector rnorm( int n /*, double mean [=0.0], double sd [=1.0] */ )
NumericVector rbeta( int n, double a, double b )
NumericVector rbinom( int n, double nin, double pp )
NumericVector rcauchy( int n, double location, double scale )
NumericVector rcauchy( int n, double location /* , double scale [=1.0] */ )
NumericVector rcauchy( int n /*, double location [=0.0] , double scale [=1.0] */ )
NumericVector rchisq( int n, double df )
NumericVector rexp( int n, double rate )
NumericVector rexp( int n /* , rate = 1 */ )
NumericVector rf( int n, double n1, double n2 )
NumericVector rgamma( int n, double a, double scale )
NumericVector rgamma( int n, double a /* scale = 1.0 */ )
NumericVector rgeom( int n, double p )
NumericVector rhyper( int n, double nn1, double nn2, double kk )
NumericVector rlnorm( int n, double meanlog, double sdlog )
NumericVector rlnorm( int n, double meanlog /*, double sdlog = 1.0 */)
NumericVector rlnorm( int n /*, double meanlog [=0.], double sdlog = 1.0 */)
NumericVector rlogis( int n, double location, double scale )
NumericVector rlogis( int n, double location /*, double scale =1.0 */ )
NumericVector rlogis( int n /*, double location [=0.0], double scale =1.0 */ )
NumericVector rnbinom( int n, double siz, double prob )
NumericVector rnbinom_mu( int n, double siz, double mu )
NumericVector rnchisq( int n, double df, double lambda )
NumericVector rnchisq( int n, double df /*, double lambda = 0.0 */ )
NumericVector rpois( int n, double mu )
NumericVector rsignrank( int n, double nn )
NumericVector rt( int n, double df )
NumericVector runif( int n, double min, double max )
NumericVector runif( int n, double min /*, double max = 1.0 */ )
NumericVector runif( int n /*, double min = 0.0, double max = 1.0 */ )
NumericVector rweibull( int n, double shape, double scale )
NumericVector rweibull( int n, double shape /* scale = 1 */ )
NumericVector rwilcox( int n, double mm, double nn )
```















```