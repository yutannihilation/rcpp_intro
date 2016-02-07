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


## Describing the Rcpp code in R code

You can also define Rcpp functions in your R source codes in 3 ways, `sourceCpp()` `cppFunction()` `evalCpp()`.

### sourceCpp()

Saving Rcpp code as string object in R and compiling it by `sourceCpp()`.

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

`cppFunction()` は、単一のRcpp関数を手軽に作成する方法を提供する。`cppFunction()`は、 R の文字列として保存された Rcpp のコードをコンパイルする。この時、`#include<Rcpp.h>`と`using namespase Rcpp`の記述を省略できる。

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

`evalCpp()` は手軽にRcppの式を評価できる

```r
# double の最大値を調べる
evalCpp('std::numeric_limits<double>::max()')



```

##C++のソースにRのコードを埋め込む

逆に Rcppのコード内に R のコードを記述することもできる。

Rcpp コード内で `/*** R`で始まるコメントの内に R のコードを書くと、`sourceCpp()` した時に、それが実行される。・


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



