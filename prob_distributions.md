# d/p/q/r 関数

Rcpp は R にある主要な全ての d/p/q/r 関数を提供する。

* d(x): density function
* p(q): cumulative distribution function
* q(p): quantile function
* r(n): random generation




一般的に `Rcpp::` 名前空間で定義されている d/p/q/r の第１引数はベクトル化されている。同じ名前の関数が `R::`名前空間の中で定義されているが、こちらはベクトル化されていない。 




Rcpp::rbeta



##beta









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