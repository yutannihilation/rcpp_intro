# List

`List` の作成と要素へのアクセスの方法は、基本的に `DataFrame` の場合と同じ。`List` はその要素として`Vector`だけではなく`S4`や `DataFrame` や `List` も保持できる。一方、`DataFrame` は、保持できる要素が、互いに長さの等しいベクターだけに制限された、 `List` の特殊な場合である。

[DataFrame](dataframe.md)のページに記載された内容は、`DataFrame` を `List`に置き換えても成立するので、詳細はそちらを参照すること。


##作成

```
List L = List::create(v1, v2); //ベクター v1, v2 からリスト L を作成
List L = List::create(Named("名前1") = v1 , _["名前2"] = v2); //要素に名前をつける場合
```

##要素へのアクセス

`List` の特定の要素にアクセスする場合には、リストの要素を ベクターに代入し、そのベクターを介してアクセスする。

`List` の要素は、数値、文字列、により指定できる。

```
NumericVector v1 = L[0];
NumericVector v2 = L["V1"];
```
