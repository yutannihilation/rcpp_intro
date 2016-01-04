# データ型

Rcppでは、Rの全ての基本的なデータ型とデータ構造を利用することができる。

##基本データ型

R、Rcpp、C++で利用できる基本的なデータ型の対応関係を「概念的」に表現すると以下のようになっている。

||R|Rcpp|C++|
|:---:|:---:|:---:|:---:|
|論理|logical|Logical|bool|
|整数|integer|Integer|int|
|数値|numeric|Numeric|double|
|複素数|complex|Complex|complex|
|文字列|character|Character (String)|string|
|日付|Date|Date|-|
|時間|POSIXct|Datetime|-|
 

なぜわざわざ「概念的」という言葉を強調したかというと、

正確には、Rcppでは `Logical`, `Integer`, `Numeric`, `Complex` については、スカラー値の型は用意されておらず、ベクター型や行列型（`NumericVector`, `NumericMatrix` など）のみが定義されている。そのため、例えば `Numeric x;` というような変数 x を宣言することはできない。


一方、 `String`, `Date`, `Datetime` についてはスカラー値の型が定義されているので、`Date d;` という変数 d が宣言できる。


# データ構造

Rcppの基本データ型のそれぞれについて`Vector`型と`Matrix`型が定義されている。


### Vector

```
IntegerVector
NumericVector
ComplexVector
CharacterVector (StringVector)
LogicalVector
DateVector
DatetimeVector
```

### Matrix

```
IntegerMatrix
NumericMatrix
ComplexMatrix
CharacterMatrix (StringMatrix)
LogicalMatrix
DateMatrix
DatetimeMatrix
```


### DataFrame, List 

```
DataFrame
List
```
`Dataframe` は同じ長さのベクターを要素として格納する。

`List`は、どのような型でも（`Dataframe` や `Listを含む`）要素に持つことができます、`Vector` の長さにも制約はありません。









