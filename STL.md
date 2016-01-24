# 標準C++が提供するアルゴリズム

標準C++のSTLライブラリでは様々な[汎用アルゴリズム](https://cpprefjp.github.io/reference/algorithm.html)が提供されている。その多くでは、イテレータを使って、アルゴリズムを適用する範囲などを指定する。イテレータについては、[イテレータの項](iterator.md)を参照のこと。

**例**：ベクターの要素の合計値を求める

```cpp
#include <Rcpp.h>
#include <numeric> //C++の標準ライブラリ
using namespace Rcpp;

// [[Rcpp::export]]
double Sum(NumericVector x) {//合計を求める
  return std::accumulate(x.begin(), x.end(), 0.0);
}
```