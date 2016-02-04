# RObject

`RObject` 型は、どんな型のオブジェクトでも代入することができる型である。どのような型が渡されるか、実行時にならないとわからない場合には、`RObject` を用いると良い。

```
void check_type(RObject)
{

}

```

`RObject` の便利な使い方として、オブジェクトの型の判別がある。




TYPEOF

メンバー関数

bool inherits(const char *clazz)