# factor

`factor` の実体は属性 `levels` が定義された `IntegerVector` である。


```
// factor の作成
// [[Rcpp::export]]
RObject rcpp_factor(){
  IntegerVector v = {1,2,3};
  CharacterVector ch = {"A","B","C"};
  v.attr("class") = "factor";
  v.attr("levels") = ch;
  return v;
}
```
```
> rcpp_factor()
[1] A B C
Levels: A B C
```



http://gallery.rcpp.org/articles/fast-factor-generation/