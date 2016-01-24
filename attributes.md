# 属性値



#### attr( name )

オブジェクトの属性値へアクセスして、値の取得や設定を行う。

```
List L;
L.attr("class") = "my_class";
```

#### attributeNames()

オブジェクトが持っている属性の一覧を返す。返値の型は C++の`vector<string>` なので、`CharacterVector`に変換するためには `wrap()` を用いる必要がある。

```
CharacterVector ch = wrap(x.attributeNames());
```
#### hasAttribute( "name" )

このオブジェクトが "name" という名前の属性を持っているかどうか。

```
bool b = x.hasAttribute("name");
```

##主要な属性値

要素名など使用頻度の高い属性については専用のアクセス関数が用意されている。

```
//要素名、これらは同義である
x.attr("names");
x.names();
```

下に主要な属性値へのアクセス方法を示す

```
//Vector V
V.names()  //要素名


//Matrix M;
M.ncol() //列数
M.nrow() //行数

M.attr(“dimnames”) = List::create(行名ベクタ, 列名ベクタ);　//行名、列名へはリストでアクセスする　

DataFrame DF;
DF.attr(“names”)     //列名
DF.attr(“row.names”) //行名

List L;
L.attr(“names”)//要素名
```



http://gallery.rcpp.org/articles/setting-object-attributes/
```cpp
// [[Rcpp::export]]
NumericVector attribs() {
  NumericVector out = NumericVector::create(1, 2, 3);

  //ベクターの要素の名前を設定する
  out.attr("names") = CharacterVector::create("a", "b", "c");
  
  //新しい属性 "my-attr" を作成して、その値に"new_attribute" をセットする。
  out.attr("new_attribute") = "my-value";
  
  //このオブジェクト class を "my-class" に設定する
  out.attr("class") = "my-class";

  return out;
}
```














