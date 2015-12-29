# NA NaN Inf


### NA
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

これらの値を条件式で用いる場合には以下の関数を用いる。

```
is_finite()
is_infinite()
is_na()
is_nan()
```

###ベクター型に代入する際の注意点


`NA_REAL` `NA_INTEGER` `NA_STRING` `NA_LOGICAL` を対応するベクターに代入すると R では NA として扱われる。


`R_NaN` `R_PosInf` `R_NegInf` は実数に対してのみ定義されている。そのため、`NumericVector` に代入された時には、`NaN` `Inf` `-Inf` として扱われるが、`IntegerVector` に代入した場合には `NA` として扱われる。




```
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

```
// [[Rcpp::export]]
List rcpp_na_scalar() {
  int    int_s = NA_INTEGER;
  double num_s = NA_REAL;
  String chr_s = NA_STRING;
  bool   lgl_s = NA_LOGICAL;
  
  return List::create(int_s, int_s+1, chr_s, lgl_s, num_s);
}
```

```
str(scalar_missings())
#> List of 4
#>  $ : int NA
#>  $ : num NA
#>  $ : chr NA
#>  $ : logi TRUE

```

`NA_LOGICAL` を C++の `bool` 型に代入した際だけ `NA` にならず `TRUE` になっている。これは、`bool`に `0` 以外の値を代入すると `true` になるが、Rcpp 内部では `NA_LOGICAL` には `int` の最小値がセットされているため。




内部表現
```
// [[Rcpp::export]]
void rcpp_na_raw(){
  Rcout << R_PosInf   << endl; //inf
  Rcout << R_NegInf   << endl; //-inf
  Rcout << R_NaN      << endl; //nan
  Rcout << NA_REAL    << endl; //nan
  Rcout << NA_INTEGER << endl; //-2147483648
  Rcout << NA_LOGICAL << endl; //-2147483648
  Rcout << NA_STRING  << endl; //0x10200dc98
}
```

C++の `double` には元々 `inf` `-inf` `nan` が定義されており、

`R_PosInf` `R_NegInf` `R_NaN` `NA_REAL` は、そのまま扱える。

一方、`NA_INTEGER` `NA_LOGICAL` には `int` の最小値がセットされており、それがRに返されるときに `NA` に変換される。



例えば、R では`NA + 1` は `NA` だが、Rcpp で `NA_INTEGER + 1` は `int` の最小値ではなくなるので、`NA` として扱われない













`NA_INTEGER` はC++内部では `int` の最小値がセットされている。RcppはこのオブジェクトをRに返すときに `NA` に変換する。




NA_LOGICAL を bool 型に代入した時だけ、挙動が異なる。



Integer
NA_INTEGER を int型 に代入すると、int 型の最小値の値を取る。Rcpp はこのオブジェクトを R に返すとき NA に変換する。しかし、C++ 中では数値として扱われるので注意が必要。



Double
C++ には NAN があるので 非数値は扱える、R の NaN に変換される。

C++ 
NAN == 1; //false、全ての論理比較は false
NAN == NAN; //false

//下には注意が必要
```
NAN && TRUE; //true
NAN || FALSE; //true

//NAN に数値演算すると NAN になる
NAN +1; //NAN



ベクタの要素がNA かどうか調べたいときは、ベクターのメソッド ::is_na() を使う。

```
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
LogicalVector is_naC(NumericVector x) {
  int n = x.size();
  LogicalVector out(n);

  for (int i = 0; i < n; ++i) {
    out[i] = NumericVector::is_na(x[i]);
  }
  return out;
}
```

あるいは suger の is_na() を使うと、論理ベクターを返す

```
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
LogicalVector is_naC2(NumericVector x) {
  return is_na(x);
}
```

```
// [[Rcpp::export]]
IntegerVector timesTwo() {
  IntegerVector y = {1,NA_INTEGER,3}; //C++11 イニシャライザリスト
  //IntegerVector::create( 1, NA_INTEGER, 3 ) ; //通常版
    //y[1] C++ では変な値が入っているので注意
   return 2*y;
}
```

> timesTwo()
[1]  2 NA  6




