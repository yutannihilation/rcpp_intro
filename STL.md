# 標準C++が提供するアルゴリズム

標準C++のSTLライブラリでは様々な[汎用アルゴリズム](https://cpprefjp.github.io/reference/algorithm.html)が提供されている。その多くでは、イテレータを使って、アルゴリズムを適用する範囲などを指定する。イテレータについては、[イテレータの項](iterator.md)を参照のこと。

**コード例**

STLアルゴリズムを使って、R の findInterval と同等の関数を作成した。
x に数値ベクター、 breaks も数値ベクターでヒストグラムなどで値の範囲を表す区切り。
x の各要素が、どの範囲（いわゆるビン）に属するか表す整数ベクターを返す。

```
#include <algorithm>
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
IntegerVector findInterval2(NumericVector x, NumericVector breaks) {
  IntegerVector out(x.size());

  NumericVector::iterator it, pos;
  IntegerVector::iterator out_it;

  for(it = x.begin(), out_it = out.begin(); it != x.end(); ++it, ++out_it) {
    pos = std::upper_bound(breaks.begin(), breaks.end(), *it);
    *out_it = std::distance(breaks.begin(), pos);
  }

  return out;
}
```

