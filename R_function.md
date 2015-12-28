# RcppからRの関数を利用する

Rcpp内でRの関数を利用するには３つの方法がある、Function、Environment、Language


##Function

Functionクラスを使うと、R の関数を引数として渡して、Rcpp内で呼び出すことができる。Rcpp内でRの関数の引数の値を指定する場は、引数の位置と名前に基づいて判断される。

引数の名前を指定する際には `Named()`または `_[]`を使用する。
`Named("引数名", "値")` でも　`Named("引数名")=値`のどちらでも良い。


例：rnorm(n, mean, sd)を受け取る関数 my_


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
上の例では、Rcppに渡されたRの関数の返り値はNumericVectorで固定れていたが、例えば、Rのlapplyのように、引数として渡される関数の返値の型が決まっていない場合もある。そのような場合にはどんな型でも代入できる RObject か List に関数の返値を代入するようにする。

**例：lapply を Rcpp で実装**

```
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
List lapply1(List input, Function f) {
  int n = input.size();
  List out(n);

  for(int i = 0; i < n; i++) {
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

`Language` クラスは R の `call()` と同様にRの関数を呼び出す事ができる。

```
Language("rnorm", )
```



