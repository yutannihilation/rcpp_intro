




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


