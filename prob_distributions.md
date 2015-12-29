# 確率分布

###分布関数

詳しくは、リファレンスを参照

d〜, p〜, q〜 の第１引数はベクトル化されている

```
p = dnorm(x, 0, 1); // 値 x における確率密度の値 p at 平均=0, 標準偏差=1
p = pnorm(x, 0, 1); // 値 x における累積分布の確率点 p
x = qnorm(p, 0, 1); // 累積分布の確率点が p となる値 x （pnorm の逆関数）
x = rnorm(n, 0, 1); // ’n’ RNG draws of N(0, 1) // ｎ個の乱数

//各関数の引数はRの関数のものと対応している
//Rcpp のリファレンスと R のヘルプを良く見比べること
double  dnorm (double x, double mu, double sigma, int lg)
//lg =1 は確率を対数で返す
pnorm (double x, double mu, double sigma, int lt, int lg)
//lt = 1 は左から確率を累積、lt = 0 は右から確率を累積
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