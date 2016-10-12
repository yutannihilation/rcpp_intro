# Defining Rcpp functions

The following code shows fundamental form of defining a Rcpp function.

```cpp
#include<Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
RETURN_TYPE FUNCTION_NAME(ARGMENT_TYPE ARGMENT){

    //your operation

    return RETURN_VALUE;
}
```

* `#include<Rcpp.h>`：Including header file so as to use Rcpp classes and functions.
* `// [[Rcpp::export]]`：The function definied just below this symbol will be exported to R.
* `using namespace Rcpp` : This statement is optional. But if you add this statement, you need not to append `Rcpp::` to every Rcpp classes and functions.
* `RETURN_TYPE FUNCTION_NAME(ARGMENT_TYPE ARGMENT){}`：You need to specify data type of return value and argments.
* `return RETURN_VALUE;`：`return` statement is nessesary
* You can use standard C++ data structure for return type and argment types. See [Standard C++ data structures](as_wrap.md) for detail.


## Embedding your Rcpp code in R code

You can also define Rcpp functions in your R source codes in 3 ways, `sourceCpp()` `cppFunction()` `evalCpp()`.

### sourceCpp()

Save Rcpp code as string object in R and compile it by `sourceCpp()`.

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

The `cppFunction()` offers the easiest way to create single Rcpp function. You can omit `#include<Rcpp.h>` and `using namespase Rcpp` in your code when using `cppFunction()`.

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

## Embedding R code code in Rcpp code

You can also embed R code in your Rcpp code. Describe your R code in a comment begining with `/*** R` and the code will be executed when you compile the code with `sourceCpp()`.

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



