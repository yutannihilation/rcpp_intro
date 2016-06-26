# NA NaN Inf NULL

ベクターの中にある NA NaN Inf の判定


####is_finite()

is.finite(v)

####is_infinite()

is_infinite(v)

####is_na()

is_na(v)

####is_nan()

is_nan(v)

####na_omit()

na_omit(v)

ベクター v から NA を削除したベクターを返す。

```
// [[Rcpp::export]]
NumericVector rcpp_na_omit(){
  NumericVector   v1  = NumericVector::create(1,2,NA_REAL,4,5);
  return na_omit(v1); // 1 2 4 5
}
```


### NULL
```
R_NilValue
```

### NA

型ごとにNA値が定義されています。

```
NA_INTEGER
NA_REAL
NA_STRING
NA_LOGICAL
```
各型のメソッド  `get_na()` を使うと、各型に対応したNA値を得ることができます。

```
IntegerVector::get_na()
NumericVector::get_na()
CharacterVector::get_na()
LogicalVector::get_na()
ComlexVector::get_na()
DataFrame::get_na();
List::get_na();
```

### NaN

```
R_NaN
```

### Inf

```
R_PosInf
R_NegInf
```

ベクターにこれらの値を与えた時に、どのように解釈されるのか、下のコード例で示す。

```cpp
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
List rcpp_na() {
  IntegerVector   v_int(3);
  NumericVector   v_num(3);
  CharacterVector v_chr(3);
  LogicalVector   v_lgl(3);
  
  v_int[0] = NA_INTEGER; //NA
  v_int[1] = R_NaN;      //NA
  v_int[2] = R_PosInf;   //NA
  
  v_num[0] = NA_REAL;    //NA
  v_num[1] = R_NaN;      //NaN
  v_num[2] = R_PosInf;   //Inf
  
  v_chr[0] = NA_STRING;  //NA
  v_chr[1] = R_NaN;      //"NaN"
  v_chr[2] = R_PosInf;   //"Inf"
  
  v_lgl[0] = NA_LOGICAL; //NA
  v_lgl[1] = R_NaN;      //NA
  v_lgl[2] = R_PosInf;   //NA
  
  return List::create(v_int, v_num, v_chr, v_lgl);
}
```

実行結果
```
> str(rcpp_na())
List of 4
 $ : int [1:3] NA NA NA
 $ : num [1:3] NA NaN Inf
 $ : chr [1:3] NA "NaN" "Inf"
 $ : logi [1:3] NA NA NA
```





##NA NaN Infの判定

ベクター v 要素にある、NA NaN Infを検出するには次の関数を使う。

```
LogicalVector l1 = is_na(v);
LogicalVector l2 = is_nan(v);
LogicalVector l3 = is_infinite(v);
LogicalVector l4 = is_finite(v);
```

```cpp
// [[Rcpp::export]]
IntegerVector rcpp_remove_na() {
   IntegerVector v = IntegerVector::create( 1, NA_INTEGER, 3 ) ;
   return v[!is_na(v)];
}
//1 3
```



１つのスカラー値についてが NA  かどうか調べたいときは、ベクターのメソッドの `Vector::is_na()`を使う。（`NumeircVector::is_na()`, `CaracterVector::is_na()` など） 

```cpp
// [[Rcpp::export]]
LogicalVector rcpp_is_naC(NumericVector x) {
  int n = x.length();
  LogicalVector out(n);
  for (int i = 0; i < n; ++i) {
    out[i] = NumericVector::is_na(x[i]);
  }
  return out;
}
```




### Rcpp の NA NaN Inf を扱う際の注意点

Rcppで定義された関数や演算子は、Rcpp の NA NaN Inf を適切に扱ってくれるが、標準C++の関数や演算子はその扱い方を必ずしも知らないので、注意が必要です。

**double**
`double` には元々 `nan` `inf` が定義されているので、Rcpp の `R_NaN` `R_PosInf`を代入すると `nan` `inf` がセットされます。`NA_REAL` は `nan ` に解釈されます。

```
double double_na  = NA_REAL;    //nan
double double_nan = R_NaN;      //nan
double double_inf = R_PosInf;   //inf
```


**int** :  `int` には `nan` `inf` が定義されていない、そのため `int` に `NA_INTEGER` `R_NaN` `R_PosInf` を代入すると `int` の最小値 `-2147483648` が設定されます。Rcpp で定義された演算では、`int` の最小値を`NA`として扱うが、標準C++ではただの数値として扱われる。

```cpp
int    int_na     = NA_INTEGER; //-2147483648
int    int_nan    = R_NaN;      //-2147483648
int    int_inf    = R_PosInf;   //-2147483648
```

下のコード例は、Rcppの演算子と標準C++の演算子における `NA_INTEGER` の扱いの違いを示しています。

```cpp
// [[Rcpp::export]]
List rcpp_na3(){
  IntegerVector   v1  = IntegerVector::create(1,NA_INTEGER,3);
  IntegerVector   v2  = IntegerVector::create(1,2,3);
  
  IntegerVector   res1(3);
  IntegerVector   res2(3);
  
  // operator+ defined by Rcpp::
  // Vector + Vector 
  res1 = v1 + v2;
  
  // operator+ defined by std::
  // int + int
  for(int i=0; i<v1.length(); ++i){
    res2[i] = v1[i] + 1;
  }
  
  // Variable "res2" no longer have NA.
  return List::create(res1, res2);
}

// [[1]]
// [1]  2  NA  4
// 
// [[2]]
// [1]  2  -2147483647  4
```


**bool** :  `bool` にも `nan` `inf` が定義されていない。`bool`に `0` 以外の値を代入すると `true` になるので、`NA_LOGICAL` `R_NaN` `R_PosInf` を代入すると、 `NA` にならず `true` になってしまう。

```
bool   bool_na    = NA_LOGICAL; //true
bool   bool_nan   = R_NaN;      //true
bool   bool_inf   = R_PosInf;   //true
```


**string**
Rcppの `String` は Rcpp で定義された型なので `NA_STRING` `R_NaN` `R_PosInf` を適切に扱うことができます。しかし、`std::string` にはこれらの値は代入できない。

```
String chr_na     = NA_STRING;  //"NA"
String chr_nan    = R_NaN;      //"NaN"
String chr_inf    = R_PosInf;   //"Inf"

//error
//std::string str_na     = NA_STRING;
//std::string str_nan    = R_NaN;
//std::string str_inf    = R_PosInf;
```

