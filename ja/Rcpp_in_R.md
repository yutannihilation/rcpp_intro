# Rのコード中にRcppのコードを埋め込む

Rcppのコードをファイルに保存せずに、Rのコードの中で記述することもできる。そのためには `sourceCpp()` `cppFunction()` `evalCpp()` を使う。

###sourceCpp()

sourceCpp() に対しては、Rcpp コードを記述したファイルへのパスを与える代わりに、RcppのコードをRの文字列として記述して与えることもできる。

```R
src<-
"#include <Rcpp.h>
using namespace Rcpp;
// [[Rcpp::export]]
double rcpp_sum(NumericVector v){
  double sum = 0;
  for(int i=0; i<v.length(); ++i){
    sum += v[i];
  }
  return(sum);
}"

sourceCpp(code = src)
rcpp_sum(1:10)
```

###cppFunction()

`cppFunction()` は、単一のRcpp関数を手軽に作成する方法を提供する。`cppFunction()`は、 R の文字列として保存された Rcpp のコードをコンパイルする。この時、`#include<Rcpp.h>`と`using namespase Rcpp`の記述を省略できる。

```r
src <-
"double rcpp_sum(NumericVector v){
  double sum = 0;
  for(int i=0; i<v.length(); ++i){
    sum += v[i];
  }
  return(sum);
}
"
Rcpp::cppFunction(src)
rcpp_sum(1:10)
```

###evalCpp

`evalCpp()` は手軽にRcppの式を評価できる

```r
# double の最大値を調べる
evalCpp('std::numeric_limits<double>::max()')



```