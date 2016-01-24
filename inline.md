

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
Rcpp のコードを R の文字列として保存し、`cppFunction()`関数を使ってコンパイルする。この場合には　`#include<Rcpp.h>`と`using namespase Rcpp`の記述を省略できるので、手軽である。

```r
code <- 
"double sum_rcpp(NumericVector v){
double sum = 0;
for(int i=0; i<v.length(); ++i){
  sum += v[i];
}
return(sum);
}"

Rcpp::cppFunction(code) //コンパイル
sum_rcpp(1:10)          //実行
```

###evalCpp

`evalCpp()` は手軽にRcppの表現を評価

```r
evalCpp('std::numeric_limits<double>::max()')



```

##C++のソースにRのコードを埋め込む

C++ コード内に、/*** R で始まるコメント内に Rのコードを書くと、sourceCpp()した時に、それが実行される。・



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

