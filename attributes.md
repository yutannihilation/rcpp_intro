# 属性値

#ベクトルの長さなど属性値

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

