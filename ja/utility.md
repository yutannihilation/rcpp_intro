# コンソール画面への出力

If you want to print massages and values of objects to R console screen, you should use `Rprintf()` or `Rcout` for standard output, `REprintf`or `Rcerr` for error output. The use of std::printf() and std::cout is not allowed for CRAN packages.

http://gallery.rcpp.org/articles/using-rcout/
https://cran.r-project.org/doc/manuals/R-exts.html#Printing

```
// [[Rcpp::export]]
void rprintf(NumericVector v){
  
  for(int i=0; i<v.length(); ++i)
    Rprintf("the value of v[%d] : %f \n", i, v[i]);
  
  Rcout << "The value of v : " << v << "\n";
  
  Rcerr << "Error message" << "\n";
  
}
```
