# 基本データ型

R、Rcpp、C++で利用できる基本的なデータ型の対応は以下のようになっている。


|R|Rcpp|C++|
|:---:|:---:|:---:|
|logical|Logical|bool|
|integer|Integer|int|
|numeric|Numeric|double|
|complex|Complex|complex|
|character|String(Character)|string|
|Date|Date|-|
|POSIXct|Datetime|-|
 

正確には、Rcppで基本データ型として定義されているのは String, Date, Datetime のみで、その他の型ついてはC++の基本型を利用する。
