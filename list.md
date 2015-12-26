#リスト

リストの作成と要素へのアクセスの方法は、基本的にデータフレームの場合と同じです。

##作成

```
List L = List::create(v1, v2); //ベクター v1, v2 からリスト L を作成
List L = List::create(Named("名前1") = v1 , _["名前2"]=v2); //要素に名前をつける場合
```

##要素へのアクセス

```
NumericVector v = L[0]; //リスト L の0番目の要素をベクター v に代入
//この場合、v には L[0] の値がコピーされるのではなく、L[0]への参照となる
v = v*2; //こうすると L[0] の値が2倍になる

//値をコピーしたい場合は clone() を使う
NumericVector v = clone(L[0]); 

```





###List を返す関数の例

```
template <typename T>
T square( const T& x){
	return x * x ;
}

// [[Rcpp::export]]
List foo2( NumericVector x ){
	return lapply( x, square<double> ) ;
}
```