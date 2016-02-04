# RObject

`RObject` 型は、どんな型のオブジェクトでも代入することができる型である。どのような型が渡されるか、実行時にならないとわからない場合には、`RObject` を用いると良い。

```
void check_type(RObject)
{

}

```

`RObject` の便利な使い方として、オブジェクトの型の判別がある。



R の C言語 API にある `TYPEOF()` はオブジェクトの `SEXPTYPE` を返す。R で定義された全ての[`SEXPTYPE`のリストはRのマニュアル](https://cran.r-project.org/doc/manuals/r-release/R-ints.html#SEXPTYPEs)を参照して欲しい。





##メンバー関数

#### inherits()
```
bool inherits(str)
```
#### slot()

```
SlotProxy slot(const std::string &name)
```
hasSlot(const std::string &name)

AttributeProxy attr(const std::string &name)

std::vector<std::string> attributeNames() const

bool hasAttribute(const std::string &name)

bool isNULL()

int sexp_type()

bool isObject() 

bool isS4() 