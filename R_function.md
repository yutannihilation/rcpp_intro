# RcppからRの関数を利用する
#Function

R の関数を引数として渡して、Rcpp内で呼び出せる

lapply を Rcpp で実装

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

関数がどんな型を返すかわからない時は、どんな型でも代入できる、RObject のオブジェクトか、List の要素に代入する。
どちらもできる。

###Function クラスのオブジェクトから、R の関数を呼び出し方。

引数を位置で指定するとき

```
f("a", 1);

```

引数の名前で指定するときは次のように書く

```
f(_["x"] = "a", _["value"] = 1);
```

