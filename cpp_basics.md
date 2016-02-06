# C++機能

## template

```
// [[Rcpp::export]]
double rcpp_sum(NumericVector v){
    int n = v.length();
    int sum = 0.0;
    for(int i=0; i<n; ++i){
        sum+=v[i];
    }
    return sum;
}

```

```


## class, struct

## 関数オブジェクト