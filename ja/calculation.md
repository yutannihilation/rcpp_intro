#四則演算と比較演算

R と同様に、`+ - * /` 演算子により、同じ長さのベクターの要素同士の四則演算を行うことができる。

```
NumericVector x ;
NumericVector y ;

NumericVector res = x + y ;
NumericVector res = x - y ;
NumericVector res = x * y ;
NumericVector res = x / y ;

NumericVector res = x   + 2.0 ;
NumericVector res = 2.0 - x;
NumericVector res = y   * 2.0 ;
NumericVector res = 2.0 / y;

NumericVector res = x * y + y / 2.0 ;
NumericVector res = x * ( y - 2.0 ) ;
NumericVector res = x / ( y * y ) ;
```

`-` 演算子は符号を反転する

```
NumericVector res = -x ;
```

##比較演算

Rと同様に、`==` `!=` `<` `>``>=` `<=`演算子を用いたベクトル同士の値の比較により、論理ベクトルを生成する。論理ベクターを使って、ベクターの要素にアクセスすることもできる。

```cpp
NumericVector x ;
NumericVector y ;

LogicalVector res = x < y ;
LogicalVector res = x > y ;
LogicalVector res = x <= y ;
LogicalVector res = x >= y ;
LogicalVector res = x == y ;
LogicalVector res = x != y ;

LogicalVector res = x < 2 ;
LogicalVector res = 2 > x;
LogicalVector res = y <= 2 ;
LogicalVector res = 2 != y;


LogicalVector res = ( x + y ) < ( x*x ) ;
LogicalVector res = ( x + y ) >= ( x*x ) ;
LogicalVector res = ( x + y ) == ( x*x ) ;
```

`!` 演算子は論理値を反転する
```
LogicalVector res = ! ( x < y );
```


論理値ベクターを使ってベクターの要素にアクセスする。

```cpp
NumericVector res = x[x < 2];
```
