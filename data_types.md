# 基本なデータ型とデータ構造

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
 

なぜわざわざ「概念的」という言葉を強調したかというと、正確には、Rcppでは `Logical`, `Integer`, `Numeric`, `Complex` については、それらのスカラー値の型は用意されておらず、ベクター型や行列型（`NumericVector`, `NumericMatrix` など）のみが定義されている。その一方で、 `Character`, `Date`, `Datetime` はスカラーの基本型として定義されている。なので、`Numeric x;` というような変数 x を宣言することはできないが、　`Date d;` という変数 d は宣言することができる。


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
Dataframe は同じ長さのベクターを要素として格納する。

リストは、どのような型でも要素に持つことができる。







