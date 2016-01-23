# 標準C++が提供するアルゴリズム

#C++ STL

STL にはイテレータを扱う様々なアルゴリズムが用意されている。
http://www.cplusplus.com/reference/algorithm/

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

