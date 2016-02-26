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



