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