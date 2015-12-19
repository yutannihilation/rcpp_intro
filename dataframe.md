# [データフレーム]

##データフレームの作成

```
DataFrame df = DataFrame::create(v1, v2); //ベクター v1, v2 からデータフレーム df を作成
DataFrame df = DataFrame::create(Named("名前1") = v1 , _["名前2"]=v2); //列に名前をつける場合
```

##要素へのアクセス

```
NumericVector v = df[0]; //dfの0列目をベクター v に代入
//この場合、v には df[0] の値がコピーされるのではなく、df[0]への参照となる
v = v*2; //こうすると df[0] の値が2倍になる
NumericVector v = clone(df[0]); //値をコピーしたい場合は clone() を使う
```


