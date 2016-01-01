# Matrix



###作成

```
NumericMatrix m(2, 3);  // 2行3列、値は0の行列
NumericMatrix m1(m);    // m の

```

###要素へのアクセス

```
m( 0 , 2 ); 0行2列の要素にアクセス
m( 0 , _ ); 0行目全体
m( _ , 2 ); 2列目全体
m( Range(0,1) , Range(2,3) ); //(0〜1)行、(2〜3)列のmの部分行列
m[0]; //mの0番目の要素
```

`m(i,j)`は行列 m の i 行 j 列への「参照」なので、m の一部の値を取得するだけではなく、m 一部に値を代入することもできる。

```
//0行目の値をベクター v にコピーする
NumericVector v = m( 0, _ ); 

//ベクター v の値を行列 m の1行目に代入する
m( 1, _ ) = v;
```
Rcppには、一部の列や行への「参照」を保持するオブジェクトも用意されている。

```
NumericMatrix::Column col = m( _ , 1); //mの1列目の値を参照したいだけの時（データのコピーが発生しない）
NumericMatrix::Row    row = m( 1 , _ ); //mの1行目の値を参照
NumericMatrix::Sub    sub = m( Range(0,1) , Range(2,3) );//mの部分行列を参照
```

m の一部への「参照」オブジェクトに対して値を代入すると、元の行列 m にその値が代入される。例えば、`col`への値の代入は `m` の列への値の代入することになる。

```
col = 2*col;      // m の 1 列目の値を2倍にすることができる
m(_,1) = 2*m(_,1) //上の例と同義
```
一方で、mの一部への参照を、ベクターに代入するとベクターに値がコピーされる。

```
//0行目の値をベクター v にコピーする
NumericVector v = m( 0, _ ); 
v = v*2; //こちらは v の値を2倍にするが m には影響しない
```




#メソッド
R_xlen_t size()
R_xlen_t nrow()
R_xlen_t ncol()

iterator begin()
iterator end()

int ncol()
int nrow()
int cols() //ncol
int rows() //nrow

Row row( int i )
Column column( int i )

void fill_diag( const U& u)

対角行列を作成
static Matrix diag( int size, const U& diag_value )

アクセス
m[int i]
m(int i, int j)
m.at(int i, int j)
m(1, _)
m(_, 1)
m(const Range& row_range, const Range& col_range)
m(const Range& row_range, _)
m(_, const Range& col_range)
m(_,_)

m.rownames("名前")
m.colnames(SEXP x)

transpose(m)
