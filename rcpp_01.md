#Rcpp入門

R の関数を C++ で記述することを可能にするパッケージ Rcpp を紹介するサイト

#開発ツールのインストール

まずは C++ のコンパイラをインストールしなければ始まりません。

Windows では [Rtools](https://cran.r-project.org/bin/windows/Rtools/index.html) をインストールします。

C:\\Rtools 以下にgccなどがインストールされる。

Mac なら Xcode をインストール

Linux なら
sudo apt-get install r-base-dev texlive-full

筆者の環境
* Windows 7 64bit
* Mac OS X 10.9.2　Xcode 5.1.1
* R 3.1.0
* RStudio 0.98.507
* Rtools 3.1
* Rcpp 0.11.1

必要に応じて（自分でインストールした gcc を使いたい場合など）、以下のファイルで環境変数を設定する。

```
$HOME/.R/Makevars
CC=/opt/local/bin/gcc-mp-4.7
CXX=/opt/local/bin/g++-mp-4.7
CPLUS_INCLUDE_PATH=$CPLUS_INCLUDE_PATH:/opt/local/include
LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/local/lib
CXXFLAGS= -g0 -O3 -Wall
MAKE=make -j4
```




#Rcpp のインストール 

Rで

```r
install.packages("Rcpp")
library(Rcpp)
```

#Rcppコードを書く


RStudio で 

File > New File > C++ File

**例：ギブス・サンプラー**

http://gallery.rcpp.org/articles/gibbs-sampler/

任意の分布関数からサンプリングされた乱数を生成するアルゴリズム。初期値からマルコフ連鎖で乱数の系列を生成する。この例では2次元で、ｘ軸方向にはガンマ分布、ｙ軸方向には正規分布に従う乱数を生成している。

２重の for ループの中で乱数を生成し、結果を行列に格納している。

```cpp
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
NumericMatrix gibbsCpp(int N, int thin) {
  
  NumericMatrix mat(N, 2);
  double x = 0, y = 0;
  
  for(int i = 0; i < N; i++) {
    for(int j = 0; j < thin; j++) {
      x = R::rgamma(3.0, 1.0 / (y * y + 4));
      y = R::rnorm(1.0 / (x + 1), 1.0 / sqrt(2 * x + 2));
    }
    mat(i, 0) = x;
    mat(i, 1) = y;
  }
  
  return(mat);
}
```



Rバージョン

```r
gibbsR <- function(N,thin){
  
  mat<-matrix(0,nrow=N,ncol=2)
  x <- 0
  y <- 0
  
  for(i in 1:N){
    for(j in 1:thin){
      x <- rgamma(1, 3, 1/(y*y+4))
      y <- rnorm(1, 1/(x+1), 1/sqrt(2*x+2))
    }
    mat[i,] <- c(x,y)
  }
  return(mat)
}
```


**コンパイル & 実行**

```r
Rcpp::sourceCpp('gibbs.cpp')
gibbsCpp(100, 10)
```

結果

```
> res <- gibbsCpp(10000, 10)
> res
[,1]        [,2]
[1,] 1.02738116  0.15099930
[2,] 0.30689307  0.50309278
[3,] 0.75111310  0.60572071
[4,] 0.55268966  0.17286207
[5,] 0.32656780  0.53070935
...
```


**R との比較**
Rcpp の方が56倍早い

```
> library(rbenchmark)
> n <- 2000
> thn <- 200
> benchmark(gibbsR(n, thn),
+           gibbsCpp(n, thn),
+           columns=c("test", "replications", "elapsed", "relative"),
+           order="relative",
+           replications=10)
test replications elapsed relative
2 gibbsCpp(n, thn)           10   1.454    1.000
1   gibbsR(n, thn)           10  81.427   56.002
```





#Boost への対応

http://gallery.rcpp.org/tags/boost/

Boost のうち、ヘッダー・ファイル・オンリーで使えるものについては、R のパッケージになっているので、R からインストールできる。

```
install.packages("BH")
```

IMM date は、毎月の３番目の水曜日
Boost には mon 月 の N 週目の M 曜日を返す関数がある。
nth_day_of_the_week_in_month(M, N, mon)

```cpp
// [[Rcpp::depends(BH)]]
#include <Rcpp.h>

// One include file from Boost
#include <boost/date_time/gregorian/gregorian_types.hpp>

using namespace boost::gregorian;

// [[Rcpp::export]]
Rcpp::Date getIMMDate(int mon, int year) {
// compute third Wednesday of given month / year
date d = nth_day_of_the_week_in_month(nth_day_of_the_week_in_month::third,
Wednesday, mon).get_date(year);
date::ymd_type ymd = d.year_month_day();
return Rcpp::wrap(Rcpp::Date(ymd.year, ymd.month, ymd.day));
}
```

```r
getIMMDate(3, 2013)
```

自分でインストールした Boost についても、ヘッダーとライブラリーへのパスを指定すればOK。
Macports でインストールした Boost なら

```
Sys.setenv("PKG_CXXFLAGS"="-std=c++11 -I/opt/local/include -L/opt/local/lib/")
```


####<a name="random">例：乱数生成器

```
#include <boost/random.hpp>
#include <boost/generator_iterator.hpp>
#include <boost/random/normal_distribution.hpp>

// [[Rcpp::export]]
NumericVector boostNormals(int n) {

typedef boost::mt19937 RNGType;   // select a generator, MT good default
RNGType rng(123456);			// instantiate and seed

boost::normal_distribution<> n01(0.0, 1.0);
boost::variate_generator< RNGType, boost::normal_distribution<> > rngNormal(rng, n01);

NumericVector V(n);
for ( int i = 0; i < n; i++ ) {
V[i] = rngNormal();
};

return V;
}
```

R, Rcpp, C++11, Boost で乱数パフォーマンス比較


Rの乱数生成器を呼び出す

```cpp
#include <Rcpp.h>
using namespace Rcpp;
// [[Rcpp::export]]
NumericVector rcppNormals(int n) {
return rnorm(n);
}
```

ベンチマーク

```
library(rbenchmark)
n <- 10000
res <- benchmark(rcppNormals(n),
boostNormals(n),
cxx11Normals(n),
rnorm(n),
order="relative",
replications = 500)
print(res[,1:4])
```

結果

```
test replications elapsed relative
2 boostNormals(n)          500   0.402    1.000
3 cxx11Normals(n)          500   0.425    1.057
1  rcppNormals(n)          500   0.505    1.256
4        rnorm(n)          500   0.675    1.679
```

C++11やBoost のネイティブ乱数生成器が早いが、Rcpp版も健闘している。どれも、ただのR関数よりは早い。

#Rcpp を学ぶための資料


Rcpp Gallery に色々サンプルがある。
Advanced R の Rcpp ページも分かりやすい
Rcppリファレンス ソースコードから自動生成されたものなのでわかりにくい

Rcpp 本家サイト
http://www.rcpp.org/

開発者のサイト
http://dirk.eddelbuettel.com/code/rcpp.html


主要な機能については Rcpp quick reference guide を参照。

その他、以下の文書を参考

Rcpp: Seamless R and C++ Integration
Rcpp Quick Reference Guide
High performance functions with Rcpp
Introduction to Rcpp Attributes


CRAN
http://cran.r-project.org/web/packages/Rcpp/index.html

このページにもRcpp の主要な使い方が簡単に紹介されている。
High performance functions with Rcpp in Advanced R programming


