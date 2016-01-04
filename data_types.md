# 基本なデータ型とデータ構造

Rcppでは、Rの全ての基本的なデータ型とデータ構造を利用することができる。


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

## Vector

Rcppの基本データ型のそれぞれについて`Vector`型と`Matrix`型が定義されている。


* `IntegerVector`
* `NumericVector`
* `ComplexVector`
* `CharacterVector` (`StringVector`)
* `LogicalVector`
* `DateVector`
* `DatetimeVector`

* `IntegerMatrix`
* `NumericMatrix`
* `ComplexMatrix`
* `CharacterMatrix` (`StringMatrix`)
* `LogicalMatrix`
* `DateMatrix`
* `DatetimeMatrix`



## データフレーム、リスト

```
Dataframe
List
```
`Dataframe` は同じ長さのベクターを要素として格納する。テーブル形式のデータ構造。

`List`は、どのような型でも（DataframeやListを含む）要素に持つことができます、ベクターの長さにも制約はありません。







