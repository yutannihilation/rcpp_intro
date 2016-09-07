# Rの関数を利用する

Rcpp 内で R の関数を利用するには、`Function`、`Environment` を用います。


##Function

Function を使ってRの関数を利用する

`Function` クラスを使うと、R の関数を引数として渡して、Rcpp 内で呼び出すことができます。R の関数に与えた値がどの引数に渡されるかは、位置と名前に基づいて判断されます。

名前を指定して引数に値を渡すには `Named()` 関数または `_[]` を使用します。`Name()` は、`Named("引数名", 値)` か `Named("引数名") = 値` の２つの方法で用いることができます。

下のコード例では、Rcpp で定義した関数の引数として R の関数 `rnorm(n, mean, sd)` を渡す例を示します。

```cpp
// [[Rcpp::export]]
NumericVector my_fun(Function f){
    // rnorm(n=5, mean=10, sd=2) と解釈されます
    // １番目の引数は位置にもとづき n に渡されます
    // ２・３番目の引数は名前にもとづき sd, mean に渡されます
    return f(5, Named("sd")=2, _["mean"]=10);
}
```

実行例

```r
#Rcpp関数に rnorm を渡します
> my_fun(rnorm)
[1]  8.014863 10.459980  7.741581  9.000762 11.465920
```

上の例では、`my_fun` に渡された R の関数の返値は `NumericVector` であると仮定されています。しかし、Rcpp 関数にどのような返値の R 関数が引数として渡されるのか決まっていない場合もよくあります。そのような場合には、どんな型でも代入できる `RObject` か `List` に関数の返値を代入するようにすると良いでしょう

下のコード例では、R の `lapply()` を単純化した関数を Rcpp で定義する例を示します。

```cpp
// [[Rcpp::export]]
List rcpp_lapply(List input, Function f) {
    // リスト input の各要素に関数 f を適用した結果をリストとして返します

    // リストの要素数 n
    R_xlen_t n = input.length();

    // 出力用に要素数が n のリストを作成します
    List out(n);

    // input の各要素に f() を適用して out に格納します
    // f() の返値の型は不明ですがリストには代入可能です
    for(R_xlen_t i = 0; i < n; ++i) {
        out[i] = f(input[i]);
    }

    return out;
}
```


## Environmentを使ってRの関数を利用する

`Environment` クラスを利用するとパッケージ等の環境からオブジェクト（変数や関数）を取り出すことができます。

下のコード例では、パッケージ `stats` にある関数 `rnorm()` 関数を呼び出す例を示します。

```cpp
// [[Rcpp::export]]
NumericVector rcpp_package_function(){
    // stat パッケージの名前空間を取得します
    Environment stats = Environment::namespace_env("stats");

    // stat パッケージの rnorm 関数を取得します
    Function rnorm = stats["rnorm"];

    // rnorm(n=5, mean=10, sd=2) を実行します
    return rnorm(Named("n", 5), Named("mean", 10), Named("sd", 2));
}
```

