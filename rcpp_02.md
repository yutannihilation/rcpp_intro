








#Rcppオブジェクトで代入する際の注意点

```
NumericVector
myFunc(NumericVector A){
   A[0]=100;
 NumericVector B=A;
 B[1]=200;
  return(A);
}//A==c(100,200,3,4,5) B への変更が A に影響する
```

これはB=Aとした時に、BにはAの値をコピーされるのではなく、BはAへの「参照」として代入されるため（AとBは同じオブジェクトを指している）。Bへのアクセスすると結局Aへアクセスすることになるため。
下のように、clone を使うと B へ A の値がコピーされる。AとBは別のオブジェクトとなる。

```
NumericVector
myFunc(NumericVector A){
   A[0]=100;
 NumericVector B=clone(A);
 B[1]=200;
  return(A);
}//A==c(100,200,3,4,5) B への変更が A に影響する
```


Rcpp のオブジェクト間で = を使うときには、「参照」として代入されているのか、「値をコピー」して代入されているのか注意する必要がある。







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


