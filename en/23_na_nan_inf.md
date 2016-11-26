# NA NaN Inf NULL


## NA NaN Inf の値

To express the value of `Inf` `-Inf` `NaN` in Rcpp, use the symbol `R_PosInf` `R_NegInf` `R_NaN`.

|R|Rcpp|
|---|---|
|`Inf|`R_PosInf`|
|`-Inf`|`R_NegInf`|
|`NaN`|`R_NaN`|

On the other hand, for `NA`, different symbol of `NA`  are defined for each `Vector` type.

| Vector | symbol of NA |
|---|---|
|`NumericVector`|`NA_REAL`|
|`IntegerVector`|`NA_INTEGER`|
|`LogicalVector`|`NA_LOGICAL`|
|`CharacterVector`|`NA_STRING`|

The following code example shows how to use these symbols to create `Vector` object.

```cpp
NumericVector   v1 = NumericVector::create( 1, NA_REAL, R_NaN, R_PosInf, R_NegInf);
IntegerVector   v2 = IntegerVector::create( 1, NA_INTEGER);
CharacterVector v3 = CharacterVector::create( "A", NA_STRING);
LogicalVector   v4 = LogicalVector::create( true, NA_LOGICAL);
```

## Evaluating NA NaN Inf

### Evaluating all the elements of a vector at once

To evaluate all the `NA` `NaN` `Inf` `-Inf` elements in a vector at once, use the function `is_na()` `is_nan()` `is_infinite()`.

In the code example below, we create a vector containing `NA` `NaN` `Inf` `-Inf` and evaluate it. From this example we can see that the `is_na()` evaluates both `NA` and` NaN` as `TRUE` (same as R's `is.na()`).

```cpp
NumericVector v =
    NumericVector::create( 1, NA_REAL, R_NaN, R_PosInf, R_NegInf);
LogicalVector l1 = is_na(v);
LogicalVector l2 = is_nan(v);
LogicalVector l3 = is_infinite(v);
Rcout << l1 << "\n"; // 0 1 1 0 0
Rcout << l2 << "\n"; // 0 0 1 0 0
Rcout << l3 << "\n"; // 0 0 0 1 1
```

You can remove `NA` `NaN` `Inf` from a vector by using these functions. You can also use `na_omit()` to remove `NA`.

The code example below shows how to remove `NA` from a vector using the `is_na()` and `na_omit()`.


```
// Creating a Vector object containg NA
NumericVector v =
    NumericVector::create( 1, NA_REAL, 2, NA_REAL, 3);

// Removeing NA from the vector
NumericVector v1 = v[!is_na(v)];
NumericVector v2 = na_omit(v);
```

### Evaluating single element of a vector

If you want to evaluate `NA` `NaN` `Inf` `-Inf` on single element of a vector, use the static member function `Vector::is_na()`, `traits::is_nan<RTYPE>()`, `traits:: is_infinite<RTYPE>()`. In `RTYPE`, specify `SEXPTYPE` of the vector to be evaluated.


```cpp
// [[Rcpp::export]]
void rcpp_is_na2() {

    // Creating Vector object containing NA NaN Inf -Inf
    NumericVector v =
        NumericVector::create(1, NA_REAL, R_NaN, R_PosInf, R_NegInf);

    // Evaluating the value for each element of the vector
    int n = v.length();
    for (int i = 0; i < n; ++i) {
        if(NumericVector::is_na(v[i]))
            Rprintf("v[%i] is NA.\n", i);
        if(Rcpp::traits::is_nan<REALSXP>(v[i]))
            Rprintf("v[%i] is NaN.\n", i);
        if(Rcpp::traits::is_infinite<REALSXP>(v[i]))
            Rprintf("v[%i] is Inf or -Inf.\n", i);
    }
}
```

Here is the list of `SEXPTYPE` of the major Vector class.

|SEXPTYPE|Vector|
|---|---|
|`LGLSXP`|LogicalVector|
|`INTSXP`|IntegerVector|
|`REALSXP`|NumericVector|
|`CPLXSXP`|ComplexVector|
|`STRSXP`|CharacterVector (StringVector)|




## NULL

Rcpp で `NULL` を扱う場合には `R_NilValue` を利用します。下のコード例では、`NULL` がリストの要素に含まれる場合の判定と、`NULL` を代入して属性の値を消去する例を示します。


```
// [[Rcpp::export]]
List rcpp_list()
{
    // 要素名がついたリストを作成する
    // ２つの要素のうち１つは NULL になっています
    List L = List::create(Named("x",NumericVector({1,2,3})),
                          Named("y",R_NilValue));

    // NULL の判定
    for(int i=0; i<L.length(); ++i){
        if(L[i]==R_NilValue) {
            Rprintf("L[%i] is NULL.\n\n", i+1);
        }
    }

    // オブジェクトの属性値（要素名）の値を消去する
    L.attr("names") = R_NilValue;

    return(L);
}
```

実行結果

```
> rcpp_list()
L[2] is NULL.

[[1]]
[1] 1 2 3

[[2]]
NULL
```




## Rcpp で NA を扱う際の注意点


内部的には `NA_INTEGER` と `NA_LOGICAL` には `int` の最小値 (-2147483648) がセットされています。Rcpp で定義された関数や演算子は `int` の最小値を `NA` として適切に扱ってくれます（つまり、`NA` の要素に対する演算の結果を `NA` にします）。しかし、標準 C++ の関数や演算子は `int` の最小値を単なる数値としてそのまま扱います。そのため、例えば `IntegerVector` の `NA` に 1 を足すと、結果は `int` の最小値ではなくなるため、もはや `NA` ではなくなってしまいます。

加えて `bool` 型に `NA` を代入した場合には常に `true` になります。これは `bool` は 0 以外の数値をすべて `true` と評価するためです。

一方、`double` には `nan` と `inf` が定義されているため、標準 C++ でも `nan` `inf` に対する演算の結果は、R と同様の結果なるため問題はありません。

下の表では、R の `NA` `Inf` `-Inf` `NaN` の値を `Vector` やスカラー型に対して代入したときに、どのような値として評価されるのかまとめています。


|                 |      NA     |     NaN     |    Inf      |    -Inf     |
|:---------------:|:-----------:|:-----------:|:-----------:|:-----------:|
| `NumericVector` | `NA_REAL`   |  `R_NaN`    | `R_PosInf`  | `R_NegInf`  |
| `IntegerVector` | `NA_INTEGER`|`NA_INTEGER` |`NA_INTEGER` |`NA_INTEGER` |
| `LogicalVector` | `NA_LOGICAL`|`NA_LOGICAL` |`NA_LOGICAL` |`NA_LOGICAL` |
|`CharacterVector`| `NA_STRING` |    "NaN"    |    "Inf"    |    "-Inf"   |
|     `String`    | `NA_STRING` |    "NaN"    |    "Inf"    |    "-Inf"   |
|     `double`    |    `nan`    |    `nan`    |    `inf`    |    `-inf`   |
|      `int`      | -2147483648 | -2147483648 | -2147483648 | -2147483648 |
|      `bool`     |    `true`   |    `true`   |    `true`   |    `true`   |


下のコード例は、`NA_INTEGER` に対して Rcpp の演算子と標準 C++ の演算子を用いて演算を行った時の結果の違いを示しています。この実行結果から Rcpp の演算子は `NA` に対する演算の結果を NA にしますが、標準 C++ の演算子は整数ベクトルの `NA` を数値として扱っていることがわかります。

```cpp
// [[Rcpp::export]]
List rcpp_na_sum(){

    // NA を含む整数ベクトルを作成します
    IntegerVector v1  = IntegerVector::create(1,NA_INTEGER,3);

    // Rcpp で定義されたベクトルとスカラーの + 演算子を適用します
    IntegerVector res1 = v1 + 1;

    // 標準 C++ で定義された int と int の + 演算子を適用します
    IntegerVector res2(3);
    for(int i=0; i<v1.length(); ++i){
        res2[i] = v1[i] + 1;
    }

    // 結果をリストで出力します
    return List::create(Named("Rcpp plus", res1),
                        Named("C++  plus", res2));
}
```

実行結果

```
> rcpp_na_sum()
$`Rcpp plus`
[1]  2 NA  4

$`C++  plus`
[1]  2 -2147483647 4
```
