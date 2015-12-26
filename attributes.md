# 属性値

#R オブジェクトの属性値にアクセスする

オブジェクト x の属性値へアクセスして、値の取得や設定を行うには `.attr()`関数を用いる。

```
x.attr("属性名")
```
要素名など使用頻度の高い属性については専用のアクセス関数が用意されている。

```
//要素名、これらは同義である
x.attr("names");
x.names();
```

下に主要な属性値へのアクセス方法を示す

```
◯◯Vector V;
V.names() //要素名
V.length() //長さ

◯◯Matrix M;
M.ncol() //列数
M.nrow() //行数
M.attr(“dimnames”)=List::create(行名ベクタ, 列名ベクタ);　//行名、列名へはリストでアクセスする　

DataFrame DF;
DF.nrows() //行数
DF.size()    //列数
DF.attr(“names”)//列名
DF.attr(“row.names”)//行名

List L;
L.attr(“names”)//要素名
```



http://gallery.rcpp.org/articles/setting-object-attributes/
```cpp
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
NumericVector attribs() {
  NumericVector out = NumericVector::create(1, 2, 3);

  //ベクターの要素の名前を設定する
  out.names() = CharacterVector::create("a", "b", "c");
  //out.attr("names") と同義
  
  //新しい属性 "my-attr" を作成して、その値に"my-value" をセットする。
  out.attr("my-attr") = "my-value";
  
  //このオブジェクト class を "my-class" に設定する
  out.attr("class") = "my-class";

  return out;
}
```

``



S4 オブジェクトに対しては .slot() が .attr() と似たような働きをする。








