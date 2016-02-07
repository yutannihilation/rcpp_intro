# コンソールへの出力

`Rprintf()` `Rcout` を使うと、Rの画面にメッセージやオブジェクトの値を出力することができる。`Rcerr`はエラー出力の際に使う。

```
// [[Rcpp::export]]
void rprintf(NumericVector v){
  
  for(int i=0; i<v.length(); ++i)
    Rprintf("the value of v[%d] : %f \n", i, v[i]);
  
  Rcout << "The value of v : " << v << "\n";
  
  Rcerr << "Error message" << "\n";
  
}
```
