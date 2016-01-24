

## Rcppコードを記述する

Rcppのコードをファイルに保存せずに、Rのコードの中で記述することもできる。`sourceCpp()` `cppFunction()` `evalCpp()` を使う。

###sourceCpp

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

###cppFunction

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

`evalCpp()` は手軽にRcppの表現を評価

```r
# double の最大値を調べる
evalCpp('std::numeric_limits<double>::max()')



```

##C++のソースにRのコードを埋め込む

逆に Rcppのコード内に R のコードを記述することもできる。

、/*** R で始まるコメント内に Rのコードを書くと、sourceCpp()した時に、それが実行される。・



```cpp
#include<Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
int fibonacci(const int x) {
    if (x < 2)
    return x;
    else
    return (fibonacci(x - 1)) + fibonacci(x - 2);
}

/*** R
# Call the ﬁbonacci function deﬁned in C++
ﬁbonacci(10)
*/
```

