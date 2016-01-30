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

```r
install.packages("RcppParallel")
install_github("RcppParallel","RcppCore")
```

Rcppソースに以下を追加
```cpp
// [[Rcpp::depends(RcppParallel)]]
#include <RcppParallel.h>
```

### parallelFor, parallelReduce

RcppParallel は `parallelFor()` と `parallelReduce()` の２つの関数を提供する。

```cpp
parallelFor(std::size_t begin, std::size_t end, 
                    Worker& worker, std::size_t grainSize = 1)
parallelReduce(std::size_t begin, std::size_t end, 
                        Reducer& reducer, std::size_t grainSize = 1)
```

`parallelFor``parallelReduce` は `begin` から `end`  までの連続した整数をインデックスとしながら `worker` `reducer` で定義された処理を並列で実行する。

`parallelFor` は入力データの各要素と出力データの各要素が１対１で対応するような処理（sqrt() や log()）を並列化する。 

`parallelReduce` は入力データの全要素を１つの値に集約するような処理（sum()やmean()）を並列化する。


### RVector, RMatrix

マルチスレッド処理では、入力データや出力データの同じ要素に対して、異なる並列スレッドが同時にアクセスすることを防ぐ "スレッドセーフ" なデータアクセスが必要がある。

`RcppParallel` では Rcppの `Vector` や　`Matrix` に対してスレッドセーフにアクセスするためのラッパー `RVector` `RMatrix`を提供している。

```cpp
IntegerVector v_int;
RVector<int> vp_int(v_int);

NumericMatrix m_num;
Rmatrix<double> mp_num(m_num);

```

## Worker, Reducer

`parallelFor` `parallelReduce` で処理する内容は関数オブジェクトとして定義する。

`parallelFor`に渡す関数オブジェクトは `Worker` を継承して作成する。同様に、`parallelReduce` に渡す関数オブジェクトは `Reduce` を継承して作成する。



## コード例

`parallelFor` を使って、`Vector` の平方根を計算する


```cpp
// [[Rcpp::depends(RcppParallel)]]
#include <RcppParallel.h>
using namespace RcppParallel;

struct SquareRoot : public Worker
{
  // 入力データを保持する内部変数
  const RMatrix<double> input_data;
  
  // 出力データを保持する内部変数
  RMatrix<double> output_data;
  
  //関数オブジェクトをインスタンス化するときに
  //入力データ・出力データを与えて内部変数を初期化する
  SquareRoot(const NumericMatrix input, NumericMatrix output) 
    : input_data(input), output_data(output) {}
  
  // 関数オブジェクトの処理内容を定義する
  // parallelFor により、ある１つのスレッドで処理する範囲が
  // begin, end で与えられる 
  void operator()(std::size_t begin, std::size_t end) {
    std::transform(input_data.begin() + begin,
                   input_data.begin() + end, 
                   output_data.begin() + begin, 
                   ::sqrt);
  }
};


// [[Rcpp::export]]
NumericMatrix parallelMatrixSqrt(NumericMatrix x) {
  
  // 出力データを保存する Matrix を用意する
  NumericMatrix output(x.nrow(), x.ncol());
  
  // 関数オブジェクトをインスタンス化する
  // このとき入力データ、出力データを渡す
  SquareRoot squareRoot(x, output);
  
  // 入力データの全ての要素に対して関数オブジェクトを適用する
  // parallelFor()の中で output に値がセットされる
  parallelFor(0, x.length(), squareRoot);
  
  // エラー：この記述は誤り parallelFor の返値は void 
  // output = parallelFor(0, x.length(), squareRoot);
  
  // 結果を出力
  return output;
}
```




```r
SquareRoot <- function( begin, end){
   sqrt_ <- function(x){
      x*x
   }
   input[begin:end]
   for(i in begin:end){
   #関数の外のoutputを書き換える(<<-)
     output[i] <<- sqrt_(input[i])
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
SquareRoot(i[1], i[3])
SquareRoot(i[4], i[6])
SquareRoot(i[7], i[10])

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