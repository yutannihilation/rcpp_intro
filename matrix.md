# Matrix

#Matrix の作成と要素へのアクセス

###作成

```
NumericMatrix m(2, 3);   2行3列、値は0の行列
```

###要素へのアクセス

```
m( 0 , 2 ); 0行2列の要素にアクセス
m( 0 , _ ); 0行目全体
m( _ , 2 ); 2列目全体
m( Range(0,1) , Range(2,3) ); //(0〜1)行、(2〜3)列のmの部分行列

NumericVector v = m( 0, _ ); //0行目の値をベクター v にコピーする

NumericMatrix::Column col = m( _ , 1); //mの1列目の値を参照したいだけの時（データのコピーが発生しない）
NumericMatrix::Row row = m( 1 , _ ); //mの1行目の値を参照
NumericMatrix::Sub sub = m( Range(0,1) , Range(2,3) );//mの部分行列を参照

col = col*2; //とすると m の 1 列目の値を2倍にすることができる
sub = sub*2; //こちらも m の (0〜1)行、(2〜3)列 の要素を2倍にする
v = v*2; //こちらは m には影響しない