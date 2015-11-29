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


#データ構造

Rcppの基本データ型のそれぞれについて

```◯◯Vector``` ```◯◯Matrix```


数値型なら、```NumericVector```、```NumericMatrix```など、文字列については、```StringVector```、```CharacterVector```のどちらの書き方でも良い。




**ベクターを要素として格納できる**

Dataframe、List







