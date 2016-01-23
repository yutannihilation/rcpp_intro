# RcppからRの関数を利用する

Rcpp内でRの関数を利用するには、`Function`、`Environment`、`Language` 、を用いる３つの方法がある。


##Function

Functionクラスを使うと、R の関数を引数として渡して、Rcpp内で呼び出すことができる。Rの関数に与えた値がどの引数に渡されるかは、位置と名前に基づいて判断される。

引数の名前を指定して値を渡すには `Named()`または `_[]`を使用する。
`Name()`の使い方は、`Named("引数名", "値")` でも　`Named("引数名")=値`のどちらでも良い。


例：Rcpp 関数に rnorm(n, mean, sd) を引数として渡す場合


```cpp
// [[Rcpp::export]]
NumericVector my_fun(Function f){
    //rnorm(n=5, mean=10, sd=2)
    //位置と名前で引数を指定する
    return f(5, Named("sd")=2, _["mean"]=10);
}

```

```r
#Rcpp関数に rnorm を渡す
my_fun(rnorm)

```
上の例では、`my_fun` に渡された R の関数の返り値は `NumericVector` であると仮定されている。しかし、どのような返値の関数が引数として渡されるのか決まっていない場合もある。そのような場合には、どんな型でも代入できる RObject か List に関数の返値を代入するようにする（下の例を参照）。

**例：lapply を Rcpp で実装**

```
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
List rcpp_lapply(List input, Function f) {
  int n = input.size();
  List out(n);

  for(int i = 0; i < n; ++i) {
    out[i] = f(input[i]);
  }

  return out;
}
```


##Environment

Environmentクラスを利用するとパッケージ等の環境からオブジェクト（変数や関数）を取り出すことができる。

```
Environment stats("package:stats"); //statsパッケージを読み込む
Function rnorm = stats["rnorm"]; //statsの環境から関数を取り出す
return f(5, Named("sd", 2), _["mean"]=10);
```


##Language

`Language` クラスを使うと、 R の `call()` と同様のやり方で R の関数を呼び出す事ができる。

```
Language call("rnorm", 5, Named("sd", ), Named("mean", 10));
call.eval();
```



