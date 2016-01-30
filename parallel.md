# RcppParallel

公式サイト：http://rcppcore.github.io/RcppParallel/


RcppParallel は Rcpp で並列プログラミングを可能にするパッケージ。バックエンドとして Windows, OS X, Linux では Intel Threaded Building Blocks (TBB) ライブラリ、その他のプラットフォームでは TinyThread ライブラリを用いている。

####他のRの並列計算パッケージとの違い

Rには既に他にも parallel や snow など、多くの並列化パッケージあるが、RcppParallel 並列化との間には重要な違いが存在する。

parallel や snow での並列化は **マルチプロセス** の方式であり、複数のRを別プロセスとして立ち上げて並列で実行する。そのため、元のRから並列計算を行うRにデータを転送する必要がある。１台のコンピュータでのみ並列計算を行う際にも並列プロセス間で　socket 通信を介してデータをコピーするため、データが大きい場合には転送に非常に時間がかかってしまう。

一方、RcppParallelでの並列化は **マルチスレッド** である。そのため、１台のコンピュータの複数コアでの並列計算しか行うことができない。しかし、並列スレッドは元のRのメモリ上のデータを共有できるため、データ転送のコストがかからない。そのため1台のPCしかない場合にはマルチスレッドのほうがアドバンテージは非常に大きくなる。

これまで、R や Rcpp のAPIを使ったマルチスレッド・プログラミングは技術的ハードルが高いため、使えるのはエキスパートに限られていた。ところが、RcppParallelを使うと、スレッド並列化に必要な処理
を全て内部で行ってくれるので、ユーザーが記述するべきコードはシンプルなもので済むようになる。手軽にマルチスレッドの恩恵を享受できる。


## インストール

```
install.packages("RcppParallel")
install_github("RcppParallel","RcppCore")
```

Rcppソースに以下を追加
```
// [[Rcpp::depends(RcppParallel)]]
#include <RcppParallel.h>
```

### parallelFor
### parallelReduce


RcppParallel は `parallelFor()` と `parallelReduce()` の２つの関数を提供する。

```
parallelFor(std::size_t begin, std::size_t end, 
                    Worker& worker, std::size_t grainSize = 1)
parallelReduce(std::size_t begin, std::size_t end, 
                        Reducer& reducer, std::size_t grainSize = 1)
```

`parallelFor``parallelReduce` は `begin` から `end`  までの連続した整数をインデックスとしながら `worker` で定義された処理を並列で実行する。

`parallelFor` は入力データの各要素と出力データの各要素が１対１で対応するような処理（sqrt() や log()）を並列化する。 

`parallelReduce` は入力データの全要素を１つの値に集約するような処理（sum()やmean()）を並列化する。


### RVector RMatrix

マルチスレッド処理では、入力データや出力データの同じ要素に対して、異なる並列スレッドが同時にアクセスすることを防ぐ "スレッドセーフ" なデータアクセスが必要がある。

`RcppParallel` では Rcppの `Vector` や　`Matrix` に対してスレッドセーフなアクセスするためのラッパー `RVector` `RMatrix`を提供している。

```
IntegerVecor
RVector<int> v_int

```


## コード例

```
// [[Rcpp::depends(RcppParallel)]]
#include <RcppParallel.h>
using namespace RcppParallel;

struct SquareRoot : public Worker
{
   // source matrix
   const RMatrix<double> input;
   
   // destination matrix
   RMatrix<double> output;
   
   // initialize with source and destination
   SquareRoot(const NumericMatrix input, NumericMatrix output) 
      : input(input), output(output) {}
   
   // take the square root of the range of elements requested
   void operator()(std::size_t begin, std::size_t end) {
      std::transform(input.begin() + begin, 
                     input.begin() + end, 
                     output.begin() + begin, 
                     ::sqrt);
   }
};
```




```
worker <- function( begin, end){
   do_something <- function(x){
      x*x
   }
   input[begin:end]
   for(i in begin:end){
   #関数の外のoutputを書き換える
     output[i] <<- do_something(input[i])
   }
   output[begin:end]
   invisible(NULL)
}



input  <- seq(10, 100, by = 10)
output <- rep(NA, 10)

begin <- 1
end <- 10

i <- begin:end

#parallelForでは次の３行は並列で実行される
worker(i[1], i[3])
worker(i[4], i[6])
worker(i[7], i[10])

print(input)
print(output)

```

parallelReduce Vector  Matrix DataFrame List の begin から end までの各要素に worker で定義された処理を適用する。



parallelFor で実行する処理



##パッケージで利用する場合


**DESCRIPTION**

```
Imports: RcppParallel
LinkingTo: RcppParallel
SystemRequirements: GNU make
```
**NAMESPACE**

```
importFrom(RcppParallel, RcppParallelLibs)
```

**src\Makevars**
```
PKG_LIBS += $(shell ${R_HOME}/bin/Rscript -e "RcppParallel::RcppParallelLibs()")
src\Makevars.win

PKG_CXXFLAGS += -DRCPP_PARALLEL_USE_TBB=1

PKG_LIBS += $(shell "${R_HOME}/bin${R_ARCH_BIN}/Rscript.exe" \
              -e "RcppParallel::RcppParallelLibs()")
```






例

http://gallery.rcpp.org/articles/parallel-matrix-transform/
http://gallery.rcpp.org/articles/parallel-vector-sum/
http://gallery.rcpp.org/articles/parallel-inner-product/
http://gallery.rcpp.org/articles/parallel-distance-matrix/