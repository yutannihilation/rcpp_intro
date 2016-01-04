# Matrix

###作成

```
NumericMatrix m(2, 3);  // matrix(0, nrow = 2, ncol = 3)
NumericMatrix m(2);     // matrix(0, nrow = 2, ncol = 2)
```

###要素へのアクセス

```
m( 0 , 2 ); //0行2列の要素にアクセス
m( 0 , _ ); //0行目全体
m( _ , 2 ); //2列目全体
m( Range(0,1) , Range(2,3) ); //(0〜1)行、(2〜3)列のmの部分行列
m[0]; //mの0番目の要素
```

`m(i,j)` を使って m の部分の値を取得するだけではなく、m 一部に値を代入することもできる。

```
//0行目の値をベクター v にコピーする
NumericVector v = m( 0, _ ); 

//ベクター v の値を行列 m の1行目に代入する
m( 1, _ ) = v;
```

###行・列・部分行列への参照
Rcppには、一部の列や行への「参照」を保持するオブジェクトも用意されている。

```
NumericMatrix::Column col = m( _ , 1);  //mの1列目の値を参照
NumericMatrix::Row    row = m( 1 , _ ); //mの1行目の値を参照
NumericMatrix::Sub    sub = m( Range(0,1) , Range(2,3) ); //mの部分行列を参照
```

m の一部への「参照」オブジェクトに対して値を代入すると、元の行列 m にその値が代入される。例えば、`col`への値の代入は `m` の列への値の代入することになる。

```
col = 2*col;      // m の 1 列目の値を2倍にすることができる
m(_,1) = 2*m(_,1) //上の例と同義
```




##メソッド

Matrix は基本的には Vector と同じメソッドを持っている。下に Matrix 固有のメソッドを示す。

#### nrow() rows()

行数

#### ncol()　cols()
列数

####row( i )

i番目の行への参照 `Vector::Row` を返す

####column( i )

i番目の列への参照 `Vector::Column` を返す

void fill_diag( const U& u)




##staticメソッド

####Matrix::diag( size, x)

行数, 列数 がsizeで、対角要素の値が x である対角行列を返す。



##その他の関数

####rownames(m)

行列 m の行名の取得と設定

```
CharacterVector ch = rownames(m);
rownames(m) = ch;
```

####colnames(m)
行列 m の列名の取得と設定
```
CharacterVector ch = colnames(m);
colnames(m) = ch;
```


####transpose(m)

行列 m の転置行列

