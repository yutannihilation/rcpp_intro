# 標準 C++ のデータ構造を利用する

標準C++には `vector` `list` `map` `set` などの様々なデータ構造が提供されている。Rcppのデータ構造に変換できるものもある。

Rcpp の `Vector` には `push_back()` `push_front()` `insert()` `erase()` など `Vector` のサイズを変更する関数が提供されている。しかし、これらを実行すると、`Vector` 全体の値をコピーする処理が行われるコストが大きい。そこで、そのような処理が必要な場合には





##データの変換

Rcpp と 標準C++のデータ構造の変換は　`as()` と `wrap()` を用いる。
Rcpp型に変換できるC++の型はRcpp関数の引数や返値にすることもできる。


* `as()` : Rcpp型 を C++型 に変換する
* `wrap()` : C++型 を Rcpp型 に変換する

| Rcpp:: | std:: |as|wrap|
|--| -- | -- | -- | -- |
| `Vector` | `vector`, `list`,`deque`  |+|+|
| `List`, `DataFrame` | `vector<vector>`, `list<deque>`, ...|+|+|
|  `Vector`(名前付き) | `map`, `unordered_map`|-|+|
| `Vector` | `set`, `unordered_set`|-|+|

標準C++のシーケンスコンテナはRcpp `Vector` と互いに変換可能。

```cpp
#include<vector>
#include<list>
#include<deque>

// [[Rcpp::export]]
void rcpp_as_wrap(){
  
  NumericVector   rcpp_vector = NumericVector::create(1,2,3,4,5);
  
  vector<double>  cpp_vector = as<vector<double> >(rcpp_vector);
  list<double>    cpp_list   = as<list<double> >(rcpp_vector);
  deque<double>   cpp_deque  = as<deque<double> >(rcpp_vector);
  
  NumericVector v1 = wrap(cpp_vector);
  NumericVector v2 = wrap(cpp_list);
  NumericVector v3 = wrap(cpp_deque);

  Rcout << v1 << "\n";
  Rcout << v2 << "\n";
  Rcout << v3 << "\n";
}

```


シーケンスコンテナの2次元コンテナ は `DataFrame`や `List` と互いに変換できる。

```cpp
// [[Rcpp::plugins("cpp11")]]

// [[Rcpp::export]]
List std_vector_2d_List(){
  using namespace std;
  //Initializing with C++11 initializer list
  vector<deque<double>> cpp_vector_2d = {{1,2},{3,4,5}};
  return wrap(cpp_vector_2d);
}

// [[Rcpp::export]]
DataFrame std_vector_2d_DataFrame(){
  using namespace std;
  vector<vector<double>> cpp_vector_2d = {{1,2},{3,4}};
  return wrap(cpp_vector_2d);
}
```

`map<key, value>` は key を名前とした、名前付き Vector に変換される。

```cpp
//#include<map>
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

## 標準C++データ構造を関数の引数や返値にする

`as()` や　`wrap()` で変換可能な標準C++データ構造は Rcpp 関数 の引数や返値にすることができる。

```
// [[Rcpp::plugins("cpp11")]]
// [[Rcpp::export]]
vector<double> times_two_std_vector(vector<double> v){
    // 暗黙的に as() が呼ばれる
    
    for(double &x : v){
        x *= 2;
    }
    return v; //暗黙的に wrap() が呼ばれる
}

```




## ユーザーが定義したクラスをRcppのデータ構造に変換する

Rcpp が対応していない C++ データ型を Rcpp のデータ構造に変換するために、ユーザーが `as()` や `wrap()` を定義することができる。

[Extending Rcpp](http://dirk.eddelbuettel.com/code/rcpp/Rcpp-extending.pdf) を参照すること









