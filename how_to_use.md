# 基本的な使い方


## Rcppコードを書く


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

