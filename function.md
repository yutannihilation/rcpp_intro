# Rcpp 関数の定義

Rcppの関数を定義する際の基本形を下に示します。

```cpp
#include<Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
返値型 関数名(引数型 引数){

    ...

    return 返値;
}
```

* `#include<Rcpp.h>`：Rcppで定義されたデータ型や関数を利用するために必要なヘッダファイルをインクルードします。
* `// [[Rcpp::export]]`：この直下に記述された関数がRで利用できるようになります。
* `using namespace Rcpp` この文は必須ではないが、これを記述すると、Rcppの変数や関数名に`Rcpp::`を付ける必要がなくなります。
* `返値型 関数名(引数型 引数){}`：C++では関数の返値や引数の型を指定する必要があります。
* `return 返値`：関数の返値は `return` 文 により明示的に指定する必要があります。
* 引数や返値として指定できる型については、[as と wrapの項](as_wrap.md)を参照。


##Rcpp コードを記述する

Rcppのコードをファイルに保存せずに、Rのコードの中で記述することもできる。`sourceCpp()` `cppFunction()` `evalCpp()` を使う。

###sourceCpp

```R
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

###cppFunction

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

###evalCpp

`evalCpp()` は手軽にRcppの表現を評価

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



