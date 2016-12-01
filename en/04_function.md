# Defining Rcpp function

The following code shows the basic format for defining a Rcpp function.

```cpp
#include<Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
RETURN_TYPE FUNCTION_NAME(ARGMENT_TYPE ARGMENT){

    //do something

    return RETURN_VALUE;
}
```

* `#include<Rcpp.h>` : This sentence enables you to use classes and functions defined by the Rcpp package.

* `// [[Rcpp::export]]`：The function defined just below this sentence will be accessible from R.

* `using namespace Rcpp;` : This sentence is optional. But if you did not write this sentence, you have to add prefix `Rcpp::` to specify classes and functions defined by the Rcpp. (For example, `Rcpp::NumericVector`)

* `RETURN_TYPE FUNCTION_NAME(ARGMENT_TYPE ARGMENT){}`：You need to specify types of functions and arguments.

* `return RETURN_VALUE;`：`return` statement is mandatory.


## Embedding Rcpp code in R code

You can also write Rcpp code in R code in 3 ways using `sourceCpp()` `cppFunction()` `evalCpp()`.

### sourceCpp()

Save Rcpp code as string object in R and compile it with `sourceCpp()`.

``` R
src<-
"#include <Rcpp.h>
using namespace Rcpp;
// [[Rcpp::export]]
double rcpp_sum(NumericVector v){
  double sum = 0;
  for(int i=0; i<v.length(); ++i){
    sum += v[i];
  }
  return(sum);
}"

sourceCpp(code = src)
rcpp_sum(1:10)
```

### cppFunction()

The `cppFunction()` offers handy way to create single Rcpp function. You can omit `#include <Rcpp.h>` and `using namespase Rcpp;` when you use `cppFunction()`.

```r
src <-
  "double rcpp_sum(NumericVector v){
    double sum = 0;
    for(int i=0; i<v.length(); ++i){
      sum += v[i];
    }
    return(sum);
  }
  "
Rcpp::cppFunction(src)
rcpp_sum(1:10)
```

### evalCpp()

You can evaluate single C++ statement by using `evalCpp()`.

```r
# Showing maximum value of double.
evalCpp('std::numeric_limits<double>::max()')
```

## Embedding R code in Rcpp code

You can embed R code in your Rcpp code. Describe your R code in a comment beginning with `/*** R` and the code will be executed when you compile the code with `sourceCpp()`.

```cpp
#include<Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
int fibonacci(const int x) {
    if (x < 2)
      return x;
    else
      return (fibonacci(x - 1)) + fibonacci(x - 2);
}

/*** R
# Call the ﬁbonacci function deﬁned in C++
ﬁbonacci(10)
*/
```
