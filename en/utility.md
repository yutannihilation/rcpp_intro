# Console output

You can print messages and object values on R console screen by using `Rprintf()` and `Rcout`. `Rcerr` can be used for error messages.

```
// [[Rcpp::export]]
void rprintf(NumericVector v){
  
  for(int i=0; i<v.length(); ++i)
    Rprintf("the value of v[%d] : %f \n", i, v[i]);
  
  Rcout << "The value of v : " << v << "\n";
  
  Rcerr << "Error message" << "\n";
  
}
```


