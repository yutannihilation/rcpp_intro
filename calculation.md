#四則演算と比較演算

R と同様に、`+ - * /` 演算子により、ベクター


同じ長さのベクターの要素同士の四則演算を行うことができる。

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
```

`-` 演算子で符号を反転する

```
NumericVector res = -x ;
```

##比較演算

Rと同様に、ベクトル同士の値の比較により、論理ベクトルを生成する。論理ベクターを使って、ベクターの要素にアクセスすることもできる。

```cpp
// two integer vectors of the same size
NumericVector x ;
NumericVector y ;
// expressions involving two vectors
LogicalVector res = x < y ;
LogicalVector res = x > y ;
LogicalVector res = x <= y ;
LogicalVector res = x >= y ;
LogicalVector res = x == y ;
LogicalVector res = x != y ;
// one vector, one single value
LogicalVector res = x < 2 ;
LogicalVector res = 2 > x;
LogicalVector res = y <= 2 ;
LogicalVector res = 2 != y;
// two expressions
LogicalVector res = ( x + y ) < ( x*x ) ;
LogicalVector res = ( x + y ) >= ( x*x ) ;
LogicalVector res = ( x + y ) == ( x*x ) ;

// ! 演算子で論理値を反転
// negate the logical expression "y < z"
LogicalVector res = ! ( x < y );
```


論理値ベクターを使って、ベクターの要素にアクセスすることもできる。

```cpp
x[x < 2];
```