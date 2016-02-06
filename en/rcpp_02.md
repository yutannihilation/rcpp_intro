
#乱数

乱数関数を使う前には RNGscope scope; のオブジェクトを作る。これはRcppが Rcpp の 関数を呼び出した Rの環境の乱数系列を受け取り、Rcpp で乱数を実行して系列を進めた後、続きの乱数系列を元のRの環境に戻すために使うらしい。RNGscope の呼び出しにはオーバーヘッドがかかるので、乱数関数の内部でそれを実装するのではなく、ユーザーが適切な位置でそれを記述することにしている。
しかし、sourceCpp() がやってくれるので実は書く必要がないみたい。

```
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
NumericMatrix rngCpp(const int N) {
  RNGScope scope;		// ensure RNG gets set/reset
  NumericMatrix X(N, 4);
  X(_, 0) = runif(N);
  X(_, 1) = rnorm(N);
  X(_, 2) = rt(N, 5);
  X(_, 3) = rbeta(N, 1, 1);
  return X;
}
```

#依存パッケージ

依存するパッケージを記述できる

```
// [[Rcpp::depends(RcppArmadillo)]]
```

複数のパッケージに依存する場合は、上のような記述を繰り返すか、下のように書く

```
// [[Rcpp::depends(Matrix, RcppArmadillo)]]
```

sourceCpp() は指定された依存パッケージに自動的にリンクする。




依存パッケージも cppFunction か evalCpp で指定できる。

```r
cppFunction(depends = 'RcppArmadillo', code = '...')
```


