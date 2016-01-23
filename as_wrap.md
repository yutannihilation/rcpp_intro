# as と wrap

標準C++には std::vector などのデータ構造が提供されており、Rcppのデータ構造に変換できるものもある。
Rcpp と 標準C++のデータ構造の変換は　`as()` と `wrap()` を用いる。

* `as()` : Rcpp型 を C++型 に変換する
* `wrap()` : C++型 を Rcpp型 に変換する



| Rcpp | C++ |
| -- | -- |
| `Vector` | `std::vector` `std::list` `std::map`|
| `List` | `std::vector< std::vector >`|



```



```



