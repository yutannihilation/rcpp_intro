# Cautions in handling Rcpp objects

### Assigning between vectors

When you assign a object `v1` to another object `v2` using `=` operator (`v2 = v1;`), the value of elements  of `v1` is not copied to `v2` but `v2` will be an alias to `v1`. Thus, if you change the value of some elements in `v1`, the change also applied to `v2`. You should use `clone()`, if you want to avoid coupling between objects (see sample code below).

The sample code presented below shows that the difference of the shallow copy and deep copy when you change value of one of vector after assigning.

```cpp
NumericVector v1 = {1,2,3};   // create a vector v1
NumericVector v2 = v1;        // v1 is assigned to v2 through shallow copy.
NumericVector v3 = clone(v1); // v1 is assigned to v3 through deep copy.

v1[0] = 100; // changing value of a element of v1

// Following output shows that
// the modification of v1 element
// is also applied to v2 but not to v3
Rcout << "v1 = " << v1 << endl; // 100 2 3
Rcout << "v2 = " << v2 << endl; // 100 2 3
Rcout << "v3 = " << v3 << endl; // 1 2 3
```
As explanation for people who have deeper knowledge of C++, a Rcpp object do not have value of R object (e.g. elements of a vector) itself, but have a pointer to R object. Thus, if you assign object through `v2 = v1;`, the value of pointer of `v1` is copied to `v2`. So, both `v1` and `v2` would be pointing to the same R object. This is called as "shallow copy". On the other hand, if you assign object through `v2 = clone(v1);`, the value of R object that `v1` is pointing is copied to `v2` as new R object. This is called "deep copy".



### 要素番号の型

32 bit システムやバージョン 2 以前の R ではベクトルの要素番号には `int` 型が使われていたため、ベクトルの要素数の最大値は 2^31 - 1 となっていました。しかし、現在一般的となっている 64 bit システムにおけるバージョン3以降のRではこれよりも要素数の大きいベクトル（Long Vector）を扱うことができます。Rcpp で Long Vector をサポートするためには、要素数や要素番号を変数として保持する場合に `int` 型ではなく `R_xlen_t` 型を用います。64 bit システムでも要素番号として `int` 型を用いることもできますが、その場合には長さが 2^31 - 1 を超えるベクトルを渡された時に処理ができなくなります。

```cpp
// 要素数 n を R_xlen_t 型として宣言する
R_xlen_t n = v.length();
double sum = 0;
// 要素番号 i を R_xlen_t 型として宣言する
for(R_xlen_t i=0; i<n; ++i){
  sum += v[i];
}
```



### []演算子の返値

`[]` や `()` 演算子でベクトルの要素へアクセスした時の返値は、`Vector`そのものではなく `Vector::Proxy` という型となっています。そのため、`v[]` を他の関数の引数として与えるとコンパイルエラーになることがあります。その場合には、`as<T>()` を用いて、目的の型 `T` (`NumericVector` など) に変換します。


```cpp
NumericVector v {1,2,3,4,5};
IntegerVector i {1,3};

// これはコンパイルエラーとなる
//double x1 = sum(v[i]);

// 変数として保持する
NumericVector vi = v[i];
double   x2 = sum(vi);

// as<T>() で変換する
double   x3 = sum(as<NumericVector>(v[i]));
```
