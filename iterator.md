# イテレーター

イテレーターとは、ベクターなどの要素にアクセスするために用意された


Rcpp のデータ構造はイテレータを持つ

```
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
double sum3(NumericVector x) {
  double total = 0;

  for(NumericVector::iterator it = x.begin(); it != x.end(); ++it) {
    total += *it;
  }
  return total;
}
```


