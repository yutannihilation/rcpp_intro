#Rcppの使い方
このページはまだ作成中なのであしからず。

DocumentはC++がわかる人向け
http://dirk.eddelbuettel.com/code/rcpp/html/






#Matrix の作成と要素へのアクセス

###作成

```
NumericMatrix m(2, 3);   2行3列、値は0の行列
```

###要素へのアクセス

```
m( 0 , 2 ); 0行2列の要素にアクセス
m( 0 , _ ); 0行目全体
m( _ , 2 ); 2列目全体
m( Range(0,1) , Range(2,3) ); //(0〜1)行、(2〜3)列のmの部分行列

NumericVector v = m( 0, _ ); //0行目の値をベクター v にコピーする

NumericMatrix::Column col = m( _ , 1); //mの1列目の値を参照したいだけの時（データのコピーが発生しない）
NumericMatrix::Row row = m( 1 , _ ); //mの1行目の値を参照
NumericMatrix::Sub sub = m( Range(0,1) , Range(2,3) );//mの部分行列を参照

col = col*2; //とすると m の 1 列目の値を2倍にすることができる
sub = sub*2; //こちらも m の (0〜1)行、(2〜3)列 の要素を2倍にする
v = v*2; //こちらは m には影響しない
```


#データフレームの作成と要素へのアクセス

```
DataFrame df = DataFrame::create(v1, v2); //ベクター v1, v2 からデータフレーム df を作成
DataFrame df = DataFrame::create(Named("名前1") = v1 , _["名前2"]=v2); //列に名前をつける場合
```

要素へのアクセス

```
NumericVector v = df[0]; //dfの0列目をベクター v に代入
//この場合、v には df[0] の値がコピーされるのではなく、df[0]への参照となる
v = v*2; //こうすると df[0] の値が2倍になる
NumericVector v = clone(df[0]); //値をコピーしたい場合は clone() を使う
```




#リストの作成と要素へのアクセス
データフレームと基本的に同じ

```
List L = List::create(v1, v2); //ベクター v1, v2 からリスト L を作成
List L = List::create(Named("名前1") = v1 , _["名前2"]=v2); //要素に名前をつける場合
```

要素へのアクセス

```
NumericVector v = L[0]; //dfの0列目をベクター v に代入
//この場合、v には L[0] の値がコピーされるのではなく、L[0]への参照となる
v = v*2; //こうすると L[0] の値が2倍になる

NumericVector v = clone(L[0]); //値をコピーしたい場合は clone() を使う
```


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

#ベクトルの長さなど属性値

```
◯◯Vector V;
V.names() //要素名
V.length() //長さ

◯◯Matrix M;
M.ncol() //列数
M.nrow() //行数
M.attr(“dimnames”)=List::create(行名ベクタ, 列名ベクタ);　//行名、列名へはリストでアクセスする　

DataFrame DF;
DF.nrows() //行数
DF.size()    //列数
DF.attr(“names”)//列名
DF.attr(“row.names”)//行名

List L;
L.attr(“names”)//要素名
```


#ベクトル演算

四則演算は R と同様の記法でOK

```
NumericVector x ;
NumericVector y ;

// expressions involving two vectors
NumericVector res = x + y ;
NumericVector res = x - y ;
NumericVector res = x * y ;
NumericVector res = x / y ;
// one vector, one single value
NumericVector res = x + 2.0 ;
NumericVector res = 2.0 - x;
NumericVector res = y * 2.0 ;
NumericVector res = 2.0 / y;
// two expressions
NumericVector res = x * y + y / 2.0 ;
NumericVector res = x * ( y - 2.0 ) ;
NumericVector res = x / ( y * y ) ;
```

-演算子で符号を反転

```
// negate x
NumericVector res = -x ;
```


#論理ベクター

数値ベクトル同士の値の比較により、論理ベクトルを生成するのも、Rと同様

```
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


論理値ベクターを使って、ベクターの要素にアクセスできる。
x[x < 2]

#Rcpp が提供する R ライクな関数


ベクター関係

```
head, tail, rep_each(), rep_len(), rep(), rev()
seq_along(), seq_len(), cumsum(), diff(), pmin(), and pmax()
```

数学関数

```
abs(), acos(), asin(), atan(), beta(), ceil(), ceiling(), choose(),cos(), cosh(), 
digamma(), exp(), expm1(), factorial(), floor(), gamma(), lbeta(),lchoose(),
lfactorial(), lgamma(), log(), log10(), log1p(), pentagamma(), psigamma(),
round(), signif(), sin(), sinh(), sqrt(), tan(), tanh(), tetragamma(), trigamma(),trunc().
```


要約統計量

```
mean(), min(), max(), sum(), sd() and (for vectors) var()
```


値の検索

```
match(), self_match(), which_max(), which_min()
```


重複の値

```
duplicated(), unique()
```


確率分布
d/q/p/r 全てのRの提供する確率分布

ベクターがNAを含まないことを保証できるなら、以下のようにすると計算が早くなる
noNA(x) 

###seq_len

NumericVector x(10);
x = seq_len(10); //1:10

###seq_along


IntegerVector x = IntegerVector::create( 0, 1, NA_INTEGER, 3 ) ;
seq_along( x ) // 1:length(x)


###rep_len

NumericVector x1 = rep_len(x, n);
all, any


bool res = is_true( any( x < y ) )
bool res = is_false( any( x < y ) )
bool res = is_na( any( x < y ) )


###NA

// [[Rcpp::export]]
IntegerVector timesTwo() {
  IntegerVector y = {1,NA_INTEGER,3}; //C++11 イニシャライザリスト
  //IntegerVector::create( 1, NA_INTEGER, 3 ) ; //通常版
    //y[1] C++ では変な値が入っているので注意
   return 2*y;
}

> timesTwo()
[1]  2 NA  6


###pmin, pmax


NumericVector z = pmin(x, y)  //x[i] と y[i] を比較して小さい方を z[i] とする
NumericVector z = pmax(x, y)  //x[i] と y[i] を比較して大きい方を z[i] とする
ifelse

x, y, z, w はベクター
ifelse(x > y, z, w);   //(x>y)[i] が真なら、z[i], 偽なら w[i] 
ifelse(x > y, 1, w);   //(x>y)[i] が真なら、1, 偽なら w[i] 


###sapply

第１引数の各要素に、第２引数で与えた関数を適用した、結果をベクターで返す

//テンプレート関数を渡す場合
template <typename T>
T square( const T& x){
    return x * x ;
}
sapply( seq_len(10), square<int> ) ;

//関数オブジェクトを与える場合
template <typename T>
struct square : std::unary_function<T,T> {
    T operator()(const T& x){
        return x * x ;
    }
}
sapply( seq_len(10), square<int>() ) ;

//std::function と ラムダ式で書く場合
 std::function<int (int)> func_obj = [](int x) { return (x*x);};
 sapply( seq_len(10), func_obj) ;


###lapply

sapplyと似ているが、返り値がlistになっている。

###sign

数値ベクターを受け取り、その各要素の符号に応じて、-1,0,1の値を格納したベクターを返す。

> sign(-3:3)
[1] -1 -1 -1  0  1  1  1


###diff

与えた数値ベクターの隣り合う要素の差を格納したベクターを返す。
y = diff(x) // y[i] == x[i+1] - x[i]

> diff(c(1,4,6))
[1] 3 2
数学関数


abs(x) //絶対値
exp(x) //eのx乗
floor(x) //xの切り下げ
ceil(x) //xの切り上げ
pow(x, 2)  //xの2乗


###分布関数

詳しくは、リファレンスを参照

d〜, p〜, q〜 の第１引数はベクトル化されている

```
p = dnorm(x, 0, 1); // 値 x における確率密度の値 p at 平均=0, 標準偏差=1
p = pnorm(x, 0, 1); // 値 x における累積分布の確率点 p
x = qnorm(p, 0, 1); // 累積分布の確率点が p となる値 x （pnorm の逆関数）
x = rnorm(n, 0, 1); // ’n’ RNG draws of N(0, 1) // ｎ個の乱数

//各関数の引数はRの関数のものと対応している
//Rcpp のリファレンスと R のヘルプを良く見比べること
double  dnorm (double x, double mu, double sigma, int lg)
//lg =1 は確率を対数で返す
pnorm (double x, double mu, double sigma, int lt, int lg)
//lt = 1 は左から確率を累積、lt = 0 は右から確率を累積
```


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


