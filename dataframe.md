# DataFrame

##オブジェクトの作成

`DataFrame` の作成には `DataFrame::create()` を使用する。また、
`DataFrame` の作成時にカラム名を指定する場合には、`Named("名前")` または `_["名前"]` を使用する。

```cpp
//ベクター v1, v2 から`DataFrame`  df を作成
DataFrame df = DataFrame::create(v1, v2); 
//列に名前をつける
DataFrame df = DataFrame::create(Named("名前1") = v1 , _["名前2"]=v2); 
```


`DataFrame` を `create()` で新たに作成する際も、`clone()` を使わないと、`DataFrame` のカラムは元のベクターの「参照」となってしまう。

下のコード例では、`df` カラム `V1` は `v` への参照となっているので、




v1は d[0], d[1], v への参照になっている。そのため、v1 を ２倍にするそへの変更は d[0], d[1], v にも影響する。　　

```
// [[Rcpp::export]]
DataFrame rcpp_df(){
  NumericVector v = NumericVector::create(1,2);
  DataFrame df = DataFrame::create( Named("V1") = v,
                                    Named("V2") = clone(v)
                                    );
  v = v*2;
  return df;
}
```
実行結果

```
> rcpp_df()
  V1 V2
1  2  1
2  4  2
```





##要素へのアクセス


`DataFrame` の特定のカラムにアクセスする場合には、カラムを一旦ベクターに代入し、そのベクターを介してアクセスする。

`DataFrame` のカラムは、数値、文字列、により指定できる。

```
NumericVector v1 = df[0];
NumericVector v2 = df["V1"];
```

上の方法で v1, v2 に df を代入すると、v1, v2 は df のカラムの値がコピーされるのではなく、df のカラムへの「参照」となる。そのため、v1, v2 への変更操作を行うとは、df のカラムの内容も変更される。



`DataFrame` のカラムをコピーして `Vector` を作成たい場合には `clone()` を用いる。

```
NumericVector v1 = df[0]; // v は dfの0列目への「参照」
v1 = v1*2;                //df[0] の値が2倍になる

NumericVector v2 = clone(df[0]); //df[0]の値をコピーする
v2 = v2*2;                       //df[0] の値は変わらない
```





##メソッド

`DataFrame` も `Vector` と同じメソッドを持っている。しかし、`Vector` の要素はスカラー値であるのに対して、 `DataFrame` の要素は `Vector` (カラム) なので、同じメソッドでも意味合いが少し変わる場合もある。



#### length() size()

列数


####nrows()

行数


####offset(str)

カラム名が str であるカラムのインデックスint

####fill(v)

この `DataFrame` の全てのカラムを ベクター v で埋める ???


####sort()

この `DataFrame` をソートしたベクターを返す ???


####assign( first_it, last_it)

イテレーター first_it, last_it で指定された範囲のカラムを、この`DataFrame` に代入する

####push_back(v)

この `DataFrame`  の末尾に `Vector` v を追加する。

####push_back( v, "col" )

この `DataFrame`  の末尾に `Vector` v を追加する
追加したカラムの名前を "col" とする。

####push_front(x)

この `DataFrame`  の先頭に `Vector` v を追加する。


####push_front( x, "x" )

この `DataFrame`  の先頭に `Vector` v を追加する
追加したカラムの名前を "col" とする。

#### begin()

この `DataFrame` の先頭カラムへのイテレータを返す。

#### end()

この `DataFrame` の末尾へのイテレータを返す。

#### insert( it, v)

この `DataFrame` の、イテレータ it で示された位置に`ベクター` v を追加し、その要素へのイテレータを返す。

#### erase(i)

この `DataFrame` の i番目のカラムを削除し、削除後の同じ位置へのイテレータを返す。

####erase(it)
イテレータ itで指定されたカラムを削除し、削除後の同じ位置の要素へのイテレータを返す。

####erase(first_i, last_i)

first_i番目 から last_i番目 までのカラムを削除し、削除後の同じ位置へのインデックスを返す。


#### erase(first_it, last_it)

first_it から last_it で指定されるカラムを削除し、削除後の同じ位置へのイテレータを返す。

#### update(df)

この `DataFrame` の内容を 他の `DataFrame`  df と同じにする

#### containsElementNamed(str)

この`DataFrame`が str で指定された名前の要素を持っているかどうかを返す

####findName(str)

指定された名前の（最初の）カラムのインデックスを返す。見つからなかったら -1 を返す。

####eval()
グローバル環境でこの`DataFrame`を評価した結果を返す

####eval(env)
環境 env でこの`DataFrame`を評価した結果を返す
