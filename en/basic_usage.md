# Basic usage

You can use your Rcpp function in 3 steps.

1. Writing Rcpp source code
2. Compiling the code
3. Exucuting the function

## Writing Rcpp source code

The below code define a function `rcpp_sum()` that calculate sum of a vector. Creating a file and save it as "sum.cpp".

```c++
//sum.cpp
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
double rcpp_sum(NumericVector v){
    double sum = 0;
    for(int i=0; i<v.length(); ++i){
        sum += v[i];
    }
    return(sum);
}
```

## Compiling the code

By using you `sourceCpp()` you cancompile the source code and load the function to R.

```R
library(Rcpp)
sourceCpp('sum.cpp')
```

## Exucuting the function

```r
> sum(1:10)
[1] 55

> rcpp_sum(1:10)
[1] 55
```




