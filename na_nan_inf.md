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



```
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
List rcpp_na() {
  IntegerVector   v_int(3);
  NumericVector   v_num(1);
  CharacterVector v_chr(1);
  LogicalVector   v_lgl(1);
  
  v_int[0] = NA_INTEGER;
  
  v_int[1] = R_NaN;
  v_int[2] = R_PosInf;
  
  v_num[0] = NA_REAL;
  v_chr[0] = NA_STRING;
  v_lgl[0] = NA_LOGICAL;
  
  return List::create(v_int, v_num, v_chr, v_lgl);
}
```





```
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
List scalar_missings() {
  int int_s = NA_INTEGER;
  String chr_s = NA_STRING;
  bool lgl_s = NA_LOGICAL;
  double num_s = NA_REAL;

  return List::create(int_s, chr_s, lgl_s, num_s);
}
```

```
str(scalar_missings())
#> List of 4
#>  $ : int NA
#>  $ : chr NA
#>  $ : logi TRUE
#>  $ : num NA
```

NA_LOGICAL を bool 型に代入した時だけ、挙動が異なる。

Integer
NA_INTEGER を int型 に代入すると、int 型の最小値の値を取る。Rcpp はこのオブジェクトを R に返すとき NA に変換する。しかし、C++ 中では数値として扱われるので注意が必要。

Double
C++ には NAN があるので 非数値は扱える、R の NaN に変換される。

C++ 
NAN == 1; //false、全ての論理比較は false
NAN == NAN; //false

//下には注意が必要
NAN && TRUE; //true
NAN || FALSE; //true

//NAN に数値演算すると NAN になる
NAN +1; //NAN

String クラスは文字列スカラーを扱うために Rcpp が用意したクラスなので NA の扱いは心得ている。

R の論理型は TRUE, FALSE, NA の値を取る、NA をC++の bool 型に変換すると true になってしまうので注意が必要。

ベクターに値としてNA を入れたい場合は、ベクターの型に合わせて NA_INTEGER,
NA_REAL
NA_STRING
NA_LOGICAL
を使う。

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



#無限の値


