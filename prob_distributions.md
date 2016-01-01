# d/p/q/r 関数

Rcpp は R にある主要な全ての d/p/q/r 関数を提供する。

* d(x): density function
* p(q): cumulative distribution function
* q(p): quantile function
* r(n): random generation


###R::とRcpp::

一般的に `Rcpp::` 名前空間で定義されている d/p/q/r の第１引数はベクトル化されている。同じ名前の関数が `R::`名前空間の中で定義されているが、こちらはベクトル化されていないので、注意すること。 

###R版との違い

`Rcpp::`名前空間で定義されている関数は、基本的にRの関数と同じ機能を持っているが、違いもある。具体的には、Rでは分布パラメータ引数（p0, p1, ...）のデフォルト値が自動的に与えられる場合でも、Rcppではユーザーが明示的に与えなければならない。

d/p/q/r関数の基本構造

ここでは分布パラメータ p0 の数は0から3まである。

Rcpp::dXXX( NumericVector x, double p0, bool log = false);
Rcpp::pXXX( NumericVector x, double p0, bool lower = true, bool log = false);
Rcpp::qXXX( NumericVector x, double p0, bool lower = true, bool log = false);
Rcpp::rXXX( int n, double p0);


NumeriVector


NumericVector Rcpp::rbeta( int n, double a, double b);


##beta

Rcpp::dbeta







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