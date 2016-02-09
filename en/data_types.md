# Data types

All the basic data types and data structures defined by R are available in Rcpp. You may think these data types/structures are implemented so as to imitate the functionalities of R's ones, however, to be more exact, Rcpp rather provide interfaces to accesing objects existing in memory kept by R.

## Basic data types

Follwing table showed "conceptual" correspondence between basic data types in R, Rcpp and standard C++.

|R|Rcpp|C++|
|:---:|:---:|:---:|:---:|
|logical|Logical|bool|
|integer|Integer|int|
|numeric|Numeric|double|
|complex|Complex|complex|
|character|Character (String)|string|
|Date|Date|-|
|POSIXct|Datetime|-|
 
Precisely, scalar data types `Logical`, `Integer`, `Numeric`, `Complex` do not exist in Rcpp, only Vector and Matrix types are defined for these data types. Thus, you cannot declare variables such as `Numeric x`, on the contrary. On the contrary, for `String`, `Date`, `Datetime`, you can declare variables like `String s`.


# Data structures

In Rcpp, `Vector` and `Matrix` are defined for each basic data types.

### Vector

```
IntegerVector
NumericVector
ComplexVector
CharacterVector (StringVector)
LogicalVector
DateVector
DatetimeVector
```

### Matrix

```
IntegerMatrix
NumericMatrix
ComplexMatrix
CharacterMatrix (StringMatrix)
LogicalMatrix
DateMatrix
DatetimeMatrix
```

### DataFrame, List 

```
DataFrame
List
```
In Rcpp, `DataFrame` and `List` are implemented as kind of vector. Namely, `DataFrame` is a vector of which elements are restricted to `Vector`s all having same sizes. On the other hand, `List` can have any data types (including `DataFrame` and `List`) as its elements, and there are no restrictions for the size of each element.




















