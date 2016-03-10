# 属性値

Rcppのオブジェクトの属性値へアクセスするには、次のメソッドを用いる。

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
#### hasAttribute( str )

このオブジェクトが 文字列 str で指定した名前の属性を持っているかどうかを返す。

```
bool b = x.hasAttribute("name");
```

```cpp
// [[Rcpp::export]]
NumericVector attribs() {
  NumericVector out = NumericVector::create(1, 2, 3);

  //ベクターの要素の名前を設定する
  out.attr("names") = CharacterVector::create("a", "b", "c");
  
  //新しい属性を作成して、その値をセットする。
  out.attr("new_attribute") = "new_value";
  
  //このオブジェクト class を "new_class" に設定する
  out.attr("class") = "new_class";

  return out;
}
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
List dimnames  = M.attr(“dimnames”);
CharacterVector rownames = dimnames[0]; //行名
CharacterVector colnames = dimnames[1]; //列名

DataFrame DF;
DF.attr(“names”)     //列名
DF.attr(“row.names”) //行名

List L;
L.attr(“names”)//要素名
```


















