# 基本なデータ型とデータ構造

Rcppでは、Rの全ての基本的なデータ型とデータ構造を利用することができます。


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
 

なぜわざわざ「概念的」という言葉を強調したかというと、正確には、Rcppでは `Logical`, `Integer`, `Numeric`, `Complex` については、スカラー値の型は用意されておらず、ベクター型や行列型（`NumericVector`, `NumericMatrix` など）のみが定義されているからです。その一方で、 `String`, `Date`, `Datetime`についてはスカラー値の型が定義されています。そのため、例えば`Numeric x;` というような変数 x を宣言することはできないですが、 `Date d;` という変数 d は宣言できます。


# データ構造

## Vector, Matrix

Rcppの基本データ型のそれぞれについて`Vector`型と`Matrix`型が定義されている。

```
◯◯Vector
◯◯Matrix
```

例えば、`NumericVector`、`NumericMatrix`など。なお、文字列ベクターについては、`CharacterVector`、`StringVector`のどちらの書き方も許されます。




## DataFrame, List

```
Dataframe
List
```
`Dataframe` は同じ長さのベクターを要素として格納する。テーブル形式のデータ構造。

`List`は、`Dataframe` や`List `を含むどのような型でも要素として持つことができます、要素であるベクターの長さにも制約はありません。







