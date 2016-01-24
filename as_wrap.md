# as と wrap

標準C++には std::vector などのデータ構造が提供されており、Rcppのデータ構造に変換できるものもある。
Rcpp と 標準C++のデータ構造の変換は　`as()` と `wrap()` を用いる。

* `as()` : Rcpp型 を C++型 に変換する
* `wrap()` : C++型 を Rcpp型 に変換する



| Rcpp:: | std:: |
| -- | -- |
| `Vector` | `vector` `list` `map`|
| `List` | `vector<vector>`|



```cpp
// [[Rcpp::export]]
NumericVector rcpp_std_map(){
  std::map<std::string, double> cpp_num_map;
  cpp_num_map["A"] = 1;
  cpp_num_map["B"] = 2;
  cpp_num_map["C"] = 3;
  return wrap(cpp_num_map);
} //Named Vector
```



