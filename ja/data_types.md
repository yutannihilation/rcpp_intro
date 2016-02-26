# データ型

Rcppでは、Rの全ての基本的なデータ型とデータ構造を利用することができる。Rcpp で提供される様々なクラスは、R のデータ型を模倣した独自のデータ構造を独自に実装しているわけではない。実際のところ、Rcppのクラスはユーザーが実行中のRのメモリにあるオブジェクトに直接アクセスして操作するためのインターフェースを提供している。

##基本データ型

R、Rcpp、C++で利用できる基本的なデータ型の対応関係を「概念的」に表現すると以下のようになっている。

||R|Rcpp|C++|
|:---:|:---:|:---:|:---:|
|論理|logical|Logical|bool|
|整数|integer|Integer|int|
|実数|numeric|Numeric|double|
|複素数|complex|Complex|complex|
|文字列|character|Character (String)|string|
|日付|Date|Date|-|
|日時|POSIXct|Datetime|-|
 

なぜわざわざ「概念的」という言葉を強調したかというと、正確には、Rcppでは `Logical`, `Integer`, `Numeric`, `Complex` については、スカラー値の型は用意されておらず、ベクター型や行列型（`NumericVector`, `NumericMatrix` など）のみが定義されている。一方、 `String`, `Date`, `Datetime` についてはスカラー型が定義されている。そのため、例えば `Numeric x;` というような変数 x を宣言することはできないが、`Date d;` という変数 d は宣言することができる。



# データ構造

Rcppの基本データ型のそれぞれについてベクター型と行列型が定義されている。以降、このドキュメントでは、Rcpp が提供するベクター型や行列型を総称するために、`Vector`, `Matrix` という語を用いる。


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


### List, DataFrame 

```
DataFrame
List
```
`Dataframe` は、様々な型のベクターを要素として格納することができる。しかし、要素となる全てのベクターサイズは等しいという制約がある。

`List`は、`Dataframe` や `List`を含む、どのような型でも要素に持つことができる。要素となる`Vector`などのサイズにも制限はない。


###Vector、DataFrame、List

Rcpp においては、`Vector`, `List`, `DataFrame` は、どれもある種のベクターとして実装されている。つまり、`Vector` は、スカラー値を要素とするベクター、`DataFrame` は同じ長さの `Vector` を要素とするベクター、`List`は任意のオブジェクトを要素するベクターである。

そのため、`Vector`, `List`, `DataFrame` は多くの共通するメンバー関数を持っている。









