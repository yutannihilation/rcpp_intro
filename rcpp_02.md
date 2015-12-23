








#Rcppオブジェクトで代入する際の注意点

```
NumericVector
myFunc(NumericVector A){
   A[0]=100;
 NumericVector B=A;
 B[1]=200;
  return(A);
}//A==c(100,200,3,4,5) B への変更が A に影響する
```

これはB=Aとした時に、BにはAの値をコピーされるのではなく、BはAへの「参照」として代入されるため（AとBは同じオブジェクトを指している）。Bへのアクセスすると結局Aへアクセスすることになるため。
下のように、clone を使うと B へ A の値がコピーされる。AとBは別のオブジェクトとなる。

```
NumericVector
myFunc(NumericVector A){
   A[0]=100;
 NumericVector B=clone(A);
 B[1]=200;
  return(A);
}//A==c(100,200,3,4,5) B への変更が A に影響する
```


Rcpp のオブジェクト間で = を使うときには、「参照」として代入されているのか、「値をコピー」して代入されているのか注意する必要がある。







#乱数

乱数関数を使う前には RNGscope scope; のオブジェクトを作る。これはRcppが Rcpp の 関数を呼び出した Rの環境の乱数系列を受け取り、Rcpp で乱数を実行して系列を進めた後、続きの乱数系列を元のRの環境に戻すために使うらしい。RNGscope の呼び出しにはオーバーヘッドがかかるので、乱数関数の内部でそれを実装するのではなく、ユーザーが適切な位置でそれを記述することにしている。
しかし、sourceCpp() がやってくれるので実は書く必要がないみたい。

```
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
NumericMatrix rngCpp(const int N) {
  RNGScope scope;		// ensure RNG gets set/reset
  NumericMatrix X(N, 4);
  X(_, 0) = runif(N);
  X(_, 1) = rnorm(N);
  X(_, 2) = rt(N, 5);
  X(_, 3) = rbeta(N, 1, 1);
  return X;
}
```







#C++ から R オブジェクトの属性にアクセスする

.attr()
.names()

```
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
NumericVector attribs() {
  NumericVector out = NumericVector::create(1, 2, 3);

  out.names() = CharacterVector::create("a", "b", "c");
  out.attr("my-attr") = "my-value";
  out.attr("class") = "my-class";

  return out;
}
```

S4 オブジェクトに対しては .slot() が .attr() と似たような働きをする。

#List と DataFrame

これらは引数として使う事が多い。
R の lm() の返り値はリストでその要素はS3オブジェクト
ここでは R の lm() の返り値を受け取る C++ 関数の例を示す。

List クラスの メンバ関数 .inherits() と stop で lm のオブジェクトかどうかチェックしている。
mod["要素名"] で リストの各要素にアクセス
as<型名>() で Rcpp クラスに変換

```
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
double mpe(List mod) {
  if (!mod.inherits("lm")) stop("Input must be a linear model");

  NumericVector resid = as<NumericVector>(mod["residuals"]);
  NumericVector fitted = as<NumericVector>(mod["fitted.values"]);

  int n = resid.size();
  double err = 0;
  for(int i = 0; i < n; ++i) {
    err += resid[i] / (fitted[i] + resid[i]);
  }
  return err / n;
}
```

```
mod <- lm(mpg ~ wt, data = mtcars)
mpe(mod)
#> [1] -0.0154
```


###List を返す関数の例

```
template <typename T>
T square( const T& x){
	return x * x ;
}

// [[Rcpp::export]]
List foo2( NumericVector x ){
	return lapply( x, square<double> ) ;
}
```

###List の生成
length, values はベクター

```
List::create(_["lengths"] = lengths, _["values"] = values)
```

#Function

R の関数を引数として渡して、Rcpp内で呼び出せる

lapply を Rcpp で実装

```
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
List lapply1(List input, Function f) {
  int n = input.size();
  List out(n);

  for(int i = 0; i < n; i++) {
    out[i] = f(input[i]);
  }

  return out;
}
```

関数がどんな型を返すかわからない時は、どんな型でも代入できる、RObject のオブジェクトか、List の要素に代入する。
どちらもできる。

###Function クラスのオブジェクトから、R の関数を呼び出し方。

引数を位置で指定するとき

```
f("a", 1);

```

引数の名前で指定するときは次のように書く

```
f(_["x"] = "a", _["value"] = 1);
```

#非数値 NaN

R_NaN

R_PosInf
R_NegInf


#欠損値 NA

NA (Not Available)

NA_INTEGER,
NA_REAL
NA_STRING
NA_LOGICAL


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


#無限の値






#イテレーター


Rcpp のデータ構造はイテレータを持つ

```
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
double sum3(NumericVector x) {
  double total = 0;

  for(NumericVector::iterator it = x.begin(); it != x.end(); ++it) {
    total += *it;
  }
  return total;
}
```

#C++ STL

STL にはイテレータを扱う様々なアルゴリズムが用意されている。
http://www.cplusplus.com/reference/algorithm/

STLアルゴリズムを使って、R の findInterval と同等の関数を作成した。
x に数値ベクター、 breaks も数値ベクターでヒストグラムなどで値の範囲を表す区切り。
x の各要素が、どの範囲（いわゆるビン）に属するか表す整数ベクターを返す。

```
#include <algorithm>
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
IntegerVector findInterval2(NumericVector x, NumericVector breaks) {
  IntegerVector out(x.size());

  NumericVector::iterator it, pos;
  IntegerVector::iterator out_it;

  for(it = x.begin(), out_it = out.begin(); it != x.end(); ++it, ++out_it) {
    pos = std::upper_bound(breaks.begin(), breaks.end(), *it);
    *out_it = std::distance(breaks.begin(), pos);
  }

  return out;
}
```

#C++のソースにRのコードを埋め込む

C++ コード内に、/*** R で始まるコメント内に Rのコードを書くと、sourceCpp()した時に、それが実行される。・

```cpp
/*** R
# Call the ﬁbonacci function deﬁned in C++
ﬁbonacci(10)
*/
```

#関数名を変える

Rで使う時の関数名を変えることができる。

```
// [[Rcpp::export(".convolveCpp")]]
NumericVector convolveCpp(NumericVector a, NumericVector b)
```

#Rにエキスポートする関数の制約

Rcpp::export する関数は、グローバルな名前空間にある必要がある。
返り値の型は void または Rcpp::wrap と適合すること、引数の型は Rcpp::as と適合すること
返り値と引数の型の指定の際には、名前空間を明示的に指定すること、つまり、vector ではなく std::vector とする。Rcpp のクラスは Rcpp:: と指定する必要はない。

#乱数生成器の注意

C や C++ から R の乱数生成器を利用する際には注意がいる。GetRNGstate と PutRNGstate を適切に呼び出さないといけない。

Rcpp ではそれを RNGScope クラスにより解決している。しかし、// [[Rcpp::export]] でエキスポートされ、sourceCpp で読み込まれた関数の場合は、それが自動的に行われるので、ソースコードに記述する必要はない。RNGScope はカウンターを使って実装されているので、仮に RNGScope を記述しても問題はない。


#依存パッケージ

依存するパッケージを記述できる

```
// [[Rcpp::depends(RcppArmadillo)]]
```

複数のパッケージに依存する場合は、上のような記述を繰り返すか、下のように書く

```
// [[Rcpp::depends(Matrix, RcppArmadillo)]]
```

sourceCpp() は指定された依存パッケージに自動的にリンクする。


#インラインで C++ を記述

R コード中で C++ を記述する方法はいくつかある。一つは文字列オブジェクトしてsourceCpp()に渡す。もう一つは、cppFunction と evalCpp を使う。

```r
cppFunction('
int fibonacci(const int x) {
    if (x < 2)
    return x;
    else
    return (fibonacci(x - 1)) + fibonacci(x - 2);
}
')
```

```r
evalCpp('std::numeric_limits<double>::max()')
```


依存パッケージも cppFunction か evalCpp で指定できる。

```r
cppFunction(depends = 'RcppArmadillo', code = '...')
```


