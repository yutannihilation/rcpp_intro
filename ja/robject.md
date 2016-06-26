# RObject

`RObject` 型は、どんな型のオブジェクトでも代入することができる型です。どのような型が渡されるか、実行時にならないとわからない場合には、`RObject` を用いると良い。

`RObject` の便利な使い方の１つとして、オブジェクトの型の判別があります。

`RObject` のメンバー関数 `sexp_type()` はこのオブジェクトの `SEXPTYPE` を返す。R で定義された全ての[`SEXPTYPE`のリストはRのマニュアル](https://cran.r-project.org/doc/manuals/r-release/R-ints.html#SEXPTYPEs)を参照して欲しい。

```cpp
// [[Rcpp::export]]
RObject check_type(RObject x){
  switch (x.sexp_type()){
  case REALSXP:
    return(wrap("numeric"));
  case INTSXP:
    return(wrap("integer"));
  case LGLSXP:
    return(wrap("logical"));
  case STRSXP:
    return(wrap("character"));
  case VECSXP:
    return(wrap("list"));
  case CPLXSXP:
    return(wrap("complex"));
  default:
    return(CharacterVector("unknown"));
  } 
}
```



##メンバー関数

`RObject` は `Vector` など Rcpp の他のデータ構造クラスと共通する以下のメンバー関数を持つ。

#### inherits()
```
bool inherits(str)
```
#### slot()

```
SlotProxy slot(const std::string &name)
```

#### hasSlot()

```
hasSlot(const std::string &name)
```

#### attr()

```
AttributeProxy attr(const std::string &name)
```

#### attributeNames()
```
std::vector<std::string> attributeNames() const
```

#### hasAttribute()
```
bool hasAttribute(const std::string &name)
```

#### isNULL()
```
bool isNULL()
```

#### sexp_type()

```
int sexp_type()
```

#### isObject()

```
bool isObject() 
```
#### isS4()

```
bool isS4() 
```
