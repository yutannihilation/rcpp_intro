# Vector

## Creating Vector object


```cpp
// v <- rep(0, 3)
NumericVector v (3);

// v <- rep(1, 3)
NumericVector v (3,1);

// v <- c(1,2,3) C++11 Initializer list
NumericVector v = {1,2,3};

// v <- c(1,2,3)
NumericVector v = NumericVector::create(1,2,3);

// v <- c(x=1, y=2, z=3)
NumericVector v =
  NumericVector::create(Named("x",1), Named("y")=2 , _["z"]=3);
```

## Accessing Vector elements

You can access to individual element of a vector object using `[]` or `()` operator. Both operators accept NumericVector/IntegerVector (numerical index), CharacterVector (element names) and LogicalVector. `[]` operator ignores out of range access, while `()` operator throws an exception.

An important point when you access to vector is that vector indices in C++ start at 0.

**[Important] : Vector indices start at 0 in C++. **


```cpp
// [[Rcpp::export]]
void rcpp_vector_access(){

  // Creating vector
  NumericVector v  {10,20,30,40,50};

  // Setting element names
  v.names() = CharacterVector({"A","B","C","D","E"});

  // Preparing vector for access
  NumericVector   numeric = {1,3};
  IntegerVector   integer = {1,3};
  CharacterVector character = {"B","D"};
  LogicalVector   logical = {false, true, false, true, false};

  // Getting values of vector elements
  double x1 = v[0];
  double x2 = v["A"];
  NumericVector res1 = v[numeric];
  NumericVector res2 = v[integer];
  NumericVector res3 = v[character];
  NumericVector res4 = v[logical];

  // Assigning values to vector elements
  v[0]   = 100;
  v["A"] = 100;
  NumericVector v2 {100,200};
  v[numeric]   = v2;
  v[integer]   = v2;
  v[character] = v2;
  v[logical]   = v2;
}
```

## Member functions

Member functions (also called as Methods) are functions that is attached to individual object. You can call member functions `f()` of object `v` in the form of `v.f()`.

```cpp
NumericVector v = {1,2,3,4,5};

// Calling member function
int n = v.length(); // 5
```

The Vector object in Rcpp have member object listed below.


### length(), size()

returns the number of elements.

### names()

returns element names as CharacterVector.

### offset(name), findName(name)

returns numerical index of the element specified by character string `name`.

### fill(x)

fills all the element of this vector with scalar value `x`.

### sort()

returns vector sorted in increasing order.


### assign( first_it, last_it)

assign values specified by iterator `first_it` and `last_it` to this vector.

### push_back(x)

append a scalar value `x` to the end of this vector.

### push_back( x, name )

append a scalar value `x` to the end of this vector and set name of the element as character string `name`.

### push_front(x)

append a scalar value `x` to the front of this vector.

### push_front( x, name )

append a scalar value `x` to the front of this vector and set name of the element as character string `name`.

### begin()

return an iterator pointing to the first element of the vector.

### end()

return an iterator pointing to the end of the vector (**one past the last element of this vector**).


### insert( i, x )

insert scalar value `x` to the position pointed by numerical index `i`. Return the iterator pointing the inserted element.


### insert( it, x )

insert scalar value `x` to the position pointed by iterator `it`. Return the iterator pointing the inserted element.

### erase(i)

erase element at the position pointed by numerical index `i`. Return the iterator pointing the element just behind the erased element.

### erase(it)

erase element at the position pointed by iterator `it`. Return the iterator pointing the element just behind the erased element.

### erase( first_i, first_i )

erase elements from the position pointed by numerical index `first_i` to `last_i - 1`. Return the iterator pointing the element just behind the erased elements.

### erase( first_it, last_it )

erase elements from the position pointed by iterator `first_it` to `last_it - 1`. Return the iterator pointing the element just behind the erased elements.

### containsElementNamed(name)

return `true` if this vector contains an element with the name specified by character string `name`.


## Static member functions

Static member function is the function that is attached to the class from which an object being molded. Static member functions is called in the form such as `NumericVector::create()`.

### get_na()

return the `NA` value of this `Vector` class.

### is_na(x)

return `true` if a vector element specified by `x` is `NA`.


### create( x1, x2, ...)

create a `Vector` object containing elements specified by scalar value `x1` and `x2`. Maximum number of arguments are 20.

### import( first_it , last_it )

create a `Vector` object filled with data from the position pointed by iterator `first_it` to `last_it - 1`.

### import_transform( first_it, last_it, func)

create a `Vector` object filled with data from the position pointed by iterator `first_it` to `last_it - 1` that is transformed by function specified by `func`.

##ベクトルを扱う際の注意点

### ベクトル同士の代入

ベクトルや行列同士で代入する際に、単純に `v2 = v1` で代入すると v1 の要素の値が v2 にコピーされるのではなく、v1 と v2 は同じオブジェクトに対する別名となります。そのため v1 の値を変更すると v2 の値も変更されてしまいます。それを避けるためには `clone()` 関数を使用します。すると v1 の値を変更しても、v2 の値は変更されません。

C++ に詳しい人のために説明すると、Rcpp のデータ型は内部にオブジェクトの値そのものではなく、オブジェクトへのポインタを保持しています。そのため、単純に `v2 = v1` で代入すると v1 が指し示すオブジェクトへのポインターの値がコピーされるので v1 と v2 は同じオブジェクトを指し示す結果となります。これをシャロー（浅い）コピーと呼びます。それに対して、`v2 = clone(v1)` を用いた場合には、`v1` が持つポインタが指し示すオブジェクトの値を複製して、新たに別のオブジェクトを作成します。これをディープ（深い）コピーと呼びます。

下のコード例では、浅いコピーと深いコピーの後に代入した元のベクトルの値を変更した時の結果の違いを示します。

```cpp
NumericVector v1 = {1,2,3};
NumericVector v2 = v1;        // v2 は単純な代入（浅いコピー）
NumericVector v3 = clone(v1); // v3 は clone() で代入（深いコピー）

v1[0] = 100; // v1 の値を変更します

// 値を確認すると v2 には v1 への変更が影響していますが
// v3 には影響していないことがわかります
Rcout << "v1 = " << v1 << endl; // 100 2 3
Rcout << "v2 = " << v2 << endl; // 100 2 3
Rcout << "v3 = " << v3 << endl; // 1 2 3
```

C++に詳しい人のために説明すると、Rcppのデータ型は内部にオブジェクトの値そのものではなく、オブジェクトへのポインタを保持しています。そのため、単純に `v2 = v1` で代入するとすると内部のポインターの値がコピーされてしまうので、このような現象が起きる。


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
