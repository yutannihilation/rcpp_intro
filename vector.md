# ベクター




###作成



```
NumericVector v(3); //c(0,0,0)
NumericVector v = NumericVector::create(1,2,3); //c(1,2,3) 
NumericVector v = NumericVector::create(Named("x") = 1 , _["y"] = 2); //c(x=1, y=2) 名前付きベクター
NumericVector v = {1,2,3}; //c(1,2,3)、C++11イニシャライザリストを使った例
```

###要素へのアクセス

```
v[0]       //要素番号（数値ベクター）でアクセス
v["名前"]  //要素の名前（文字列ベクター）でアクセス
v[L]       //論理値ベクター(L)でアクセス
```

RcppはC++のスタイルに従い、ベクター要素のインデックスは０から始まる。


```
//例：ベクターvの総和を計算する
double sum=0;
for(int i=0; i<v.length(); ++i){
    sum += v[i];
}
```


