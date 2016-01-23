# イテレーター

イテレーターとは、ベクターなどの要素にアクセスするためのオブジェクトである。Rcpp のデータ構造は、それぞれ独自のイテレータ型が定義されている。


```
// [[Rcpp::export]]
double rcpp_sum(NumericVector x) {
  double total = 0;
  for(NumericVector::iterator it = x.begin(); it != x.end(); ++it) {
    total += *it;
  }
  return total;
}
```

```
// [[Rcpp::export]]

```


`push_back()` `erase()`などによる要素の追加や削除後は、

イテレータが保持している値は


