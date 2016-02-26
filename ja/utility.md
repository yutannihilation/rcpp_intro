# コンソール画面への出力

メッセージやオブジェクトの値を画面に表示するためには `Rprintf()` か `Rcout` を用いる。エラー表示のためには `REprintf` か `Rcerr` を用いる。`Rprintf()`の使い方はC++標準にある `printf` と同じである。

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



http://gallery.rcpp.org/articles/using-rcout/
https://cran.r-project.org/doc/manuals/R-exts.html#Printing
