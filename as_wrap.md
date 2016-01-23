# as と wrap

Rcpp と 標準C++のデータ構造の変換は　`as()` と `wrap()` を用いる。

* `as()` : C++型 を Rcpp型 に変換する
* `wrap()` : Rcpp型 を C++型 に変換する

標準C++には std::vector などのデータ構造が提供されており、Rcppのデータ構造に変換できるものもある。Rcppのデータ型に変換できる標準C++データ型は、Rcpp関数の引数や返値とすることもできる。

| Rcpp | C++ |
| -- | -- |
| `Vector` | `std::vector` `std::list` `std::map`|
|||



