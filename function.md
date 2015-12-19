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
