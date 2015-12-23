# 関数の定義

Rcppの関数を定義する際の基本形を下に示す。

```cpp
#include<Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
返値型 関数名(引数型 引数){

    ...

    return(返値);
}
```

* `#include<Rcpp.h>`：Rcppで定義されたデータ型や関数を利用するために必要なヘッダファイルをインクルードする
* `// [[Rcpp::export]]`：この直下に記述された関数がRで利用できるようになる。
* `using namespace Rcpp` Rcpp：この文は必須ではないが、これを記述すると、Rcppの変数や関数名に`Rcpp::`を付ける必要がなくなる。
* `返値型 関数名(引数型 引数){}`：C++では関数の返値や引数の型を指定する必要がある。
* `return(返値)`：関数の返値は `return()` により明示的に指定する必要がある
* 


##Rにエキスポートする関数の制約

 * `//[[Rcpp::export]]` する関数は、グローバルな名前空間にある必要がある。
 * 返値の型は Rcpp::wrap() によりRcpp型に変換可能な



引数の型は Rcpp::as と適合すること

返り値と引数の型の指定の際には、名前空間を明示的に指定すること、つまり、vector ではなく std::vector とする。Rcpp のクラスは Rcpp:: と指定する必要はない。



###関数名を変える

Rで使う時の関数名を変えることができる。

```
// [[Rcpp::export(".convolveCpp")]]
NumericVector convolveCpp(NumericVector a, NumericVector b)
```