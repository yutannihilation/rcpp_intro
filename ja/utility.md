# コンソール画面への出力

メッセージやオブジェクトの値を画面に表示するためには `Rprintf()` か `Rcout` を用います。エラー表示のためには `REprintf()` か `Rcerr` を用います。`Rprintf()` `REprintf()` の使い方はC++標準にある `printf` と同じです。

```cpp
// [[Rcpp::export]]
void rprintf(NumericVector v){
  
  for(int i=0; i<v.length(); ++i){
    Rprintf("the value of v[%d] : %f \n", i, v[i]);
  }
  
  Rcout << "The value of v : " << v << "\n";
  Rcerr << "Error message" << "\n";
  
}
```
