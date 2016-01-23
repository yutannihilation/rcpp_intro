# イテレーター

イテレーターとは、ベクターなどの要素にアクセスするためのオブジェクトである。Rcpp のデータ構造は、それぞれ独自のイテレータ型が定義されている。

```
NumericVector::iterator   nm_i;
CharacterVector::iterator ch_i;
...
DataFrame::iterator       df_i;
List::iterator            lt_i;
```

* `i = v.begin()` とするとイテレータ `i` には `v` の先頭要素を指し示す状態にセットされる。
* `*i` は、イテレータが指し示す要素の値を表す。
* `i+2` は、iの2つ次の要素を指し示すイテレータ
* `i-1` は、iの1つ前の要素を指し示すイテレータ
* `++i` iを1つ次の要素を指す状態に更新する
* `--i` iを1つ前の要素を指す状態に更新する
* `v.end()` は `v` の末尾（最後の要素の１つ後）を指し示すイテレータ

![](iterator.png)

```cpp
// [[Rcpp::export]]
void rcpp_iterator(){
  CharacterVector v = CharacterVector::create("A","B","C","D");
  CharacterVector::iterator i;
  i = v.begin();
  Rcout << *i     << endl;  //"A"
  Rcout << *(i+1) << endl;  //"B"
  Rcout << *(++i) << endl;  //"B"
  Rcout << *(++i) << endl;  //"C"
}
```

例；イテレータを使って、`NumericVector`の要素の値の合計値を求める

```cpp
// [[Rcpp::export]]
double rcpp_sum(NumericVector x) {
  double total = 0;
  for(NumericVector::iterator it = x.begin(); it != x.end(); ++it) {
    total += *it;
  }
  return total;
}
```


基本的には、`push_back()` `erase()`などにより、ベクターの要素の追加や削除を行った後には、それまでイテレータが指し示していた状態は無効になるものと思っているのが安全である。


