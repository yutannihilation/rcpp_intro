# 基本データ型

R、Rcpp、C++で利用できる基本的なデータ型の概念的な対応は以下のようになっている。

||R|Rcpp|C++|
|:---:|:---:|:---:|:---:|
|論理|logical|Logical|bool|
|整数|integer|Integer|int|
|数値|numeric|Numeric|double|
|複素数|complex|Complex|complex|
|文字列|character|Character (String)|string|
|日付|Date|Date|-|
|時間|POSIXct|Datetime|-|
 



正確には、Rcppでは `Logical`, `Integer`, `Numeric`, `Complex` については基本型として存在しているわけではなく、それらのベクター型、行列型のみが Rcpp では定義されている。一方、 `Character`, `Date`, `Datetime` は基本型として定義されている。


そのため、`Numeric x;` というような変数 x を宣言することはできないが、　`Date d` という変数は宣言することができる。


# データ構造

## ベクター

Rcppの基本データ型のそれぞれについてベクター型と行列型が定義されている。

```
◯◯Vector
◯◯Matrix
```

数値型なら、`NumericVector`、`NumericMatrix`など、文字列については、`CharacterVector`、`StringVector`のどちらの書き方も許される。




## データフレーム、リスト

```
Dataframe
List
```
Rと同様に Dataframe は同じ長さのベクターを要素として格納する。一方、リストはデータフレームやリストを含むあらゆる型をその要素に持つことができる。






