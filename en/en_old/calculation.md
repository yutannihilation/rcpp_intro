#Arithmetic and comparison operations

##Arithmetic operations

Like as R, operator `+ - * /` would execute element-wise calculation for `Vector` and `Matrix`.

```
NumericVector x ;
NumericVector y ;

// operation between two vectors
NumericVector res = x + y ;
NumericVector res = x - y ;
NumericVector res = x * y ;
NumericVector res = x / y ;

// operation between scalar and vector
NumericVector res = x   + 2.0 ;
NumericVector res = 2.0 - x;
NumericVector res = y   * 2.0 ;
NumericVector res = 2.0 / y;
```

operator `-` flips signs of vector/matrix elements.

```
NumericVector res = -x ;
```

##Comparison operations

Like as R, conparison operation with vector/matrices would generates `LogicalVector`/`Logicalmatrix`.

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


// operator `!` negates values of `LogicalVector` elements.

```
LogicalVector res = ! ( x < y );
```

You can use `LogicalVector`/`Logicalmatrix` to acccess subset of `Vector`/`Matrix` elements.

```cpp
x[x < 2];
```