# [データフレーム]

##データフレームの作成

データフレームの作成には `DataFrame::create()` を使用する。また、
データフレームの作成時にカラム名を指定する場合には、`Named("名前")` または `_["名前"]` を使用する。

```
DataFrame df = DataFrame::create(v1, v2); //ベクター v1, v2 からデータフレーム df を作成
DataFrame df = DataFrame::create(Named("名前1") = v1 , _["名前2"]=v2); //列に名前をつける場合
```



##要素へのアクセス


データフレームの特定のカラムにアクセスする場合には、カラムを一旦ベクターに代入し、そのベクターを介してアクセスする。

データフレームのカラムは、数値、文字列、により指定できる。

```
NumericVector v1 = df[0];
NumericVector v2 = df["V1"];
```
df のカラムをベクターに代入した場合の振る舞いは、dfが関数内部で作成されたのか、関数の外から引数として与えられたのか、により振る舞いが異なる。


** df が関数内部で作成されたオブジェクトである場合**

上の方法で v1, v2 に df を代入すると、v1, v2 は df のカラムの値がコピーされるのではなく、df のカラムへの「参照」となる。そのため、v1, v2 への変更は、df のカラムも変更される。

データフレームのカラムをコピーしたい場合には `clone()` を用いる。

```
NumericVector v1 = df[0]; // v は dfの0列目への「参照」
v1 = v1*2;                //df[0] の値が2倍になる

NumericVector v2 = clone(df[0]); //df[0]の値をコピーする
v2 = v2*2;                       //df[0] の値は変わらない
```

データフレームを `create()` で新たに作成する際も、`clone()` を使わないと、データフレームのカラムは元のベクターの「参照」となってしまう。


```
// [[Rcpp::export]]
DataFrame rcpp_df1(){
  NumericVector v = NumericVector::create(1,2);
  DataFrame df = DataFrame::create( Named("V1") = v,
                                    Named("V2") = v,
                                    Named("V3") = clone(v)
                                    );
  // v1 は df[0] への参照となる
  NumericVector v1 = df[0]; 
  v1 = v1*2; 
  return df;
}
```
実行結果

```
> rcpp_df1()
  V1 V2 V3
1  2  2  1
2  4  4  2
```
上の例では、v1は d[0], d[1], v への参照になっているので、v1への変更は d[0], d[1], v にも影響する。　　


** df が引数の場合**

df が引数の場合は、dfのカラムをベクターに代入しても df への参照とはならずに、カラムの値がコピーされる。

```
// [[Rcpp::export]]
DataFrame rcpp_df2(DataFrame df){
  // df が引数の場合
  // v1 は df[0] への参照にはならず、
  // df[0] の値がコピーされる
  NumericVector v1 = df[0];
  v1 = v1*2;
  return df; //v1への変更は、dfには影響しない
}
```

```
df <- data.frame(1:5, 6:10)
rcpp_df2(df)
```


基本的には、Rcpp関数に引数として渡された R のオブジェクトが、Rcpp関数内部での操作により変更されてしまうことはないが、例外もある。

Rcppで記述した関数に data.frame を引数として与えると、Rcpp関数内部における操作により、
引数として与えた元のRのデータフレーム・オブジェクトが変更されてしまうことがある。

```cpp
// [[Rcpp::export]]
void rcpp_df3(DataFrame df){
  NumericVector v1 = df[0];
  df[0] = v1*2;
}
```

```R
df <- data.frame(V1=1:5, V2=6:10)
rcpp_df3(df)
print(df)
```
```
  V1 V2
1  2  6
2  4  7
3  6  8
4  8  9
5 10 10

```





##メソッド

`DataFrame` も `Vector` と同じメソッドを持っている。しかし、`Vector` の要素はスカラー値であるのに対して、 `DataFrame` の要素は `Vector` (カラム) なので、



下に、`DataFrame` 固有のメソッドを示す。

#### length() size()

列数


####nrows()

行数



