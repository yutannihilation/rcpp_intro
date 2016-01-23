# NA NaN Inf


### NA

Rcpp では型ごとに異なるNA値が定義されている。

```
NA_INTEGER
NA_REAL
NA_STRING
NA_LOGICAL
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

###値の判定

```
is_finite()
is_infinite()
is_na()
is_nan()
```

###ベクター型に代入する


`NA_REAL` `NA_INTEGER` `NA_STRING` `NA_LOGICAL` を対応するベクターに代入すると R では NA として扱われる。


`R_NaN` `R_PosInf` `R_NegInf` は実数に対してのみ定義されている。そのため、`NumericVector` に代入された時には、`NaN` `Inf` `-Inf` として扱われるが、`IntegerVector` に代入した場合には `NA` として扱われる。




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






###スカラー型に代入した場合の挙動


スカラー型に代入した場合の挙動


```cpp
// [[Rcpp::export]]
List rcpp_na_scalar() {

  double double_na  = NA_REAL;    //nan
  double double_nan = R_NaN;      //nan
  double double_inf = R_PosInf;   //inf
  
  int    int_na     = NA_INTEGER; //-2147483648
  int    int_nan    = R_NaN;      //-2147483648
  int    int_inf    = R_PosInf;   //-2147483648
  
  String chr_na     = NA_STRING;  //"NA"
  String chr_nan    = R_NaN;      //"NaN"
  String chr_inf    = R_PosInf;   //"Inf"
  
  bool   bool_na    = NA_LOGICAL; //true
  bool   bool_nan   = R_NaN;      //true
  bool   bool_inf   = R_PosInf;   //true
  
  return(List::create(
      Named("double_na" , double_na),
      Named("double_nan", double_nan),
      Named("double_inf", double_inf),
      
      Named("int_na" , int_na),
      Named("int_nan", int_nan),
      Named("int_inf", int_inf),
      
      Named("String_na" , chr_na),
      Named("String_nan", chr_nan),
      Named("String_inf", chr_inf),
      
      Named("bool_na" , bool_na),
      Named("bool_nan", bool_nan),
      Named("bool_inf", bool_inf)
  ));
}

```

```
> str(rcpp_na_scalar())
List of 12
 $ int_na    : int NA
 $ int_nan   : int NA
 $ int_inf   : int NA
 $ double_na : num NA
 $ double_nan: num NaN
 $ double_inf: num Inf
 $ String_na : chr NA
 $ String_nan: chr "NaN"
 $ String_inf: chr "Inf"
 $ bool_na   : logi TRUE
 $ bool_nan  : logi TRUE
 $ bool_inf  : logi TRUE
```

C++ の `double` には元々 `nan` `inf` が定義されているので、Rcpp の `R_NaN` `R_PosInf` をそのまま扱うことができる。

`int` には `nan` `inf` が定義されていない、そのため `int` に `R_NaN` `R_PosInf` を代入すると `NA` になる。また、Rcppでは `int` の最小値を`NA`として扱う、`int` の最小値をRに返すときに`NA`に変換される。しかし、C++の中では数値として扱われるので注意すること。

Rcppの `String` は `NA_STRING` `R_NaN` `R_PosInf` を適切に扱うことができる。

C++の `bool` 型に`NA_LOGICAL` を代入すると `NA` にならず `TRUE` になってしまうので注意する。これは、`bool`に `0` 以外の値を代入すると `true` になるが、Rcpp 内部では `NA_LOGICAL` には `int` の最小値がセットされているため。




## 値の比較

C++におけるNaNの挙動

```cpp
//NANとの全ての論理比較は false
NAN == 1;   //false
NAN == NAN; //false

//NAN と bool のAND, OR 演算はTRUEになる
NAN && TRUE;  //true
NAN || FALSE; //true

//NAN と数値の演算は NAN になる
NAN +1; //NAN
```


ベクターの１つの要素がNA かどうか調べたいときは、ベクターのメソッド `is_na()` を使う。`NumeircVector::is_na()` 

```cpp
// [[Rcpp::export]]
LogicalVector rcpp_is_naC(NumericVector x) {
  LogicalVector out(x.size());
  for (int i = 0; i < x.size(); ++i) {
    out[i] = NumericVector::is_na(x[i]);
  }
  return out;
}
```


suger の `is_na()` を使うと、ベクターのすべての要素を判定した結果を論理ベクターとして返す。

```cpp
// [[Rcpp::export]]
IntegerVector filter_na() {
   IntegerVector v = IntegerVector::create( 1, NA_INTEGER, 3 ) ;
   return v[!is_na(v)];
}
```
実行結果
```
> filter_na()
[1] 1 3
```




