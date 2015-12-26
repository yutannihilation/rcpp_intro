#リスト

リストの作成と要素へのアクセスの方法は、基本的にデータフレームの場合と同じです。

##作成

```
List L = List::create(v1, v2); //ベクター v1, v2 からリスト L を作成
List L = List::create(Named("名前1") = v1 , _["名前2"]=v2); //要素に名前をつける場合
```

##要素へのアクセス

```
NumericVector v = L[0]; //リスト L の0番目の要素をベクター v に代入
//この場合、v には L[0] の値がコピーされるのではなく、L[0]への参照となる
v = v*2; //こうすると L[0] の値が2倍になる

//値をコピーしたい場合は clone() を使う
NumericVector v = clone(L[0]); 

```


## Listを引数として受け取る関数の例



ここでは lm() の返値を引数として受け取る Rcpp 関数の例を示す。lm() の返値はS3オブジェクトだがその正体はリストである。

**このコードは Advanced R のコピペなので要対応**

```cpp
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
double mpe(List mod) {
  if (!mod.inherits("lm")) stop("Input must be a linear model");

  NumericVector resid  = mod["residuals"];
  NumericVector fitted = mod["fitted.values"];

  int n = resid.size();
  double err = 0;
  for(int i = 0; i < n; ++i) {
    err += resid[i] / (fitted[i] + resid[i]);
  }
  return err / n;
}
```

```
mod <- lm(mpg ~ wt, data = mtcars)
mpe(mod)
#> [1] -0.0154
```


###List を返す関数の例

```
template <typename T>
T square( const T& x){
	return x * x ;
}

// [[Rcpp::export]]
List foo2( NumericVector x ){
	return lapply( x, square<double> ) ;
}
```