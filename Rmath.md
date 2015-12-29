# R Mathematics Library

 R Mathematics Library はRが提供する数学や統計などの関数を提供するライブラリであり、Rmath.h ヘッダーに記述されている。Rmath 自体は独立したライブラリーとして機能するので、他のプログラムからでも利用できる。

Rcpp からも Rmath.h で定義された関数を呼び出す事ができる。それらは `R::` 名前空間の中で定義されている。

Rmath.h の関数はC言語で書かれており、ベクトル化されていない。Rcppはベクトル化された関数を提供しているので、一般的なユーザーはそちらを使うと良いだろう。



```c
/* Random Number Generators */
double norm_rand(void);
double unif_rand(void);
double exp_rand(void);

/* Normal Distribution */
double dnorm(double x, double mu, double sigma, int lg);
double pnorm(double x, double mu, double sigma, int lt, int lg);
double qnorm(double p, double mu, double sigma, int lt, int lg);
double rnorm(double mu, double sigma);
void   pnorm_both(double x, double *cum, double *ccum, int lt, int lg);

...

```

