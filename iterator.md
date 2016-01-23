# イテレーター

イテレーターとは、ベクターなどの要素にアクセスするためのオブジェクトである。Rcpp のデータ構造は、それぞれ独自のイテレータ型が定義されている。

```
NumericVector::iterator   nm_i;
CharacterVector::iterator ch_i;
DataFrame::iterator       df_i;
List::iterator            lt_i;
...
```

`i = v.begin()` とするとイテレータ i には v の先頭要素を指し示す値がセットされる。

この時、`*i` は、イテレータが指し示す要素の値を表す

```cpp
CharacterVector v = CharacterVector::create("A","B","C","D");
CharacterVector::iterator i;

i = v.begin();
Rcout << *i << endl;     //"A"
Rcout << *(i+1) << endl; //"B"
Rcout << v[1]   << endl; //"B"



bool 





```
![](iterator.png)

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

```
// [[Rcpp::export]]

```


基本的には、`push_back()` `erase()`などによる要素の追加や削除の後には、それまでイテレータが保持していた値は無効になるものと思っているのが安全である。


