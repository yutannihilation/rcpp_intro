# as と wrap

標準C++には std::vector などのデータ構造が提供されており、Rcppのデータ構造に変換できるものもある。
Rcpp と 標準C++のデータ構造の変換は　`as()` と `wrap()` を用いる。
Rcpp型に変換できるC++の型はRcpp関数の引数や返値にすることもできる。


* `as()` : Rcpp型 を C++型 に変換する
* `wrap()` : C++型 を Rcpp型 に変換する

| Rcpp:: | std:: |as|wrap|
|--| -- | -- | -- | -- |
| `Vector` | `array`, `deque`, `list`, `vector` |+|+|
| `List`, `DataFrame` | `vector<vector>`, `list<deque>`, ...|+|+|
| Named `Vector` | `map`, `unordered_map`|-|+|
| `Vector` | `set`, `unordered_set`|-|+|


```cpp
// [[Rcpp::export]]
NumericVector rcpp_as_wrap(){
  using namespace std;
  
  NumericVector   rcpp_vector  = NumericVector::create(1,2,3,4,5);

  //OK
  //array<double, 5> cpp_container = as<array<double,5> >(rcpp_vector);
  //vector<double> cpp_container = as<vector<double> >(rcpp_vector);
  //deque<double> cpp_container = as<deque<double> >(rcpp_vector);
  list<double> cpp_container = as<list<double> >(rcpp_vector);
  
  
  return wrap(cpp_container);
}
```




`map<key, value>` は key を名前とした、名前付き Vector に変換される。

```cpp
// [[Rcpp::export]]
NumericVector std_map(){
  std::map<std::string, double> cpp_num_map;
  cpp_num_map["A"] = 1;
  cpp_num_map["B"] = 2;
  cpp_num_map["C"] = 3;
  return wrap(cpp_num_map);
}
//Named Vector

```
2次元コンテナ は `DataFrame`や `List` と互いに変換できる。

```cpp
// [[Rcpp::plugins("cpp11")]]

// [[Rcpp::export]]
List std_vector_2d_List(){
  using namespace std;
  //Initializing with C++11 initializer list
  vector<vector<double>> cpp_vector_2d = {{1,2},{3,4,5}};
  return wrap(cpp_vector_2d);
}

// [[Rcpp::export]]
DataFrame std_vector_2d_DataFrame(){
  using namespace std;
  vector<vector<double>> cpp_vector_2d = {{1,2},{3,4}};
  return wrap(cpp_vector_2d);
}
```


