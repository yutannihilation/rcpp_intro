# factor

`factor` の実体は属性 `levels` が定義された `IntegerVector` です。

下の例では、`IntergerVector` に属性を指定することで `factor` に変換する例を示しています。 `factor` 型では整数ベクトルの値 1 が属性 `levels` （文字列ベクトル） の 1 番目 （C++では 0 番目） の要素に対応し、同様に、整数ベクトルの値 2 が属性 `levels` （文字列ベクトル） の 2 番目 （C++では 1 番目） の要素に対応します。

```
// factor の作成
// [[Rcpp::export]]
RObject rcpp_factor(){
  IntegerVector v = {1,2,3,1,2,3};
  CharacterVector ch = {"A","B","C"};
  v.attr("class") = "factor";
  v.attr("levels") = ch;
  return v;
}
```

下の実行結果を見ると R に返された整数ベクトル v は factor 型として扱われていることがわかります。

```
> rcpp_factor()
[1] A B C A B C
Levels: A B C
```



