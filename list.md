#リスト
##リストの作成
データフレームと基本的に同じ

```
List L = List::create(v1, v2); //ベクター v1, v2 からリスト L を作成
List L = List::create(Named("名前1") = v1 , _["名前2"]=v2); //要素に名前をつける場合
```

##要素へのアクセス

```
NumericVector v = L[0]; //dfの0列目をベクター v に代入
//この場合、v には L[0] の値がコピーされるのではなく、L[0]への参照となる
v = v*2; //こうすると L[0] の値が2倍になる

NumericVector v = clone(L[0]); //値をコピーしたい場合は clone() を使う

```


#List と DataFrame

これらは引数として使う事が多い。
R の lm() の返り値はリストでその要素はS3オブジェクト
ここでは R の lm() の返り値を受け取る C++ 関数の例を示す。

List クラスの メンバ関数 .inherits() と stop で lm のオブジェクトかどうかチェックしている。
mod["要素名"] で リストの各要素にアクセス
as<型名>() で Rcpp クラスに変換

```
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
double mpe(List mod) {
  if (!mod.inherits("lm")) stop("Input must be a linear model");

  NumericVector resid = as<NumericVector>(mod["residuals"]);
  NumericVector fitted = as<NumericVector>(mod["fitted.values"]);

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