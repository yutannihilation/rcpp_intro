# RcppからRの関数を利用する

Rcpp内でRの関数を利用するには３つの方法がある、Function、Environment、Language


##Function

Functionクラスを使うと、R の関数を引数として渡して、Rcpp内で呼び出すことができる。

rnorm()を受け取る関数を作成する。

```
// [[Rcpp::export]]
NumericVector my_fun(Function f){

    

}

```



Rcpp内でRの関数の引数に値を与える場合には、引数の位置と名前に基づいて判断される。

引数を位置で指定するとき

```
f("a", 1);

```

引数の名前で指定するときは`Named("引数名")`または`_["引数名"]`を利用する。

```
f(_["x"] = "a", _["value"] = 1);
```


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
lapplyのように、Rcpp関数に引数として渡されるRの関数の返値の型が不定である場合には、どんな型でも代入できる、RObject か List に返値を代入するようにする。
