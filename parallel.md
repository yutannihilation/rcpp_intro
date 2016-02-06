# RcppParallel

公式サイト：http://rcppcore.github.io/RcppParallel/


RcppParallel は Rcpp で並列プログラミングを可能にするパッケージ。バックエンドとして Windows, OS X, Linux では Intel Threaded Building Blocks (TBB) ライブラリ、その他のプラットフォームでは TinyThread ライブラリを用いている。

####他の R の並列計算パッケージとの違い

Rには既に他にも parallel や snow など、多くの並列化パッケージあるが、RcppParallel 並列化との間には重要な違いが存在する。

parallel や snow での並列化は **マルチプロセス** の方式であり、複数のRを別プロセスとして立ち上げて並列で実行する。そのため、元のRから並列計算を行うRにデータを転送する必要がある。１台のコンピュータでのみ並列計算を行う際にも並列プロセス間で　socket 通信を介してデータをコピーするため、データが大きい場合には転送に非常に時間がかかってしまう。

一方、RcppParallelでの並列化は **マルチスレッド** である。そのため、１台のコンピュータの複数コアでの並列計算しか行うことができない。しかし、並列スレッドは元のRとメモリ上のデータを共有できるため、データ転送のコストがかからない。そのため1台のPCしかない場合にはマルチスレッドのほうがアドバンテージは非常に大きくなる。

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
void parallelFor(std::size_t begin, std::size_t end, 
                    Worker& worker, std::size_t grainSize = 1)
void parallelReduce(std::size_t begin, std::size_t end, 
                        Reducer& reducer, std::size_t grainSize = 1)
```

`parallelFor``parallelReduce` は `Vector`と `Matrix` の `begin` から `end-1` までの要素に対して `worker` `reducer` で定義された処理を並列で実行する。

**parallelFor** は入力データの各要素と出力データの各要素が１対１で対応するような処理（sqrt() や log()）を並列化する場合に用いる。

```
//parallelForは次のような処理を並列化する
for(int i=begin; i<end; ++i){
    output[i] = f(input[i])
}
```

**parallelReduce** は入力データの全要素を１つの値に集約するような処理（sum()やmean()）を並列化する場合に用いる。

```
//parallelReduceは次のような処理を並列化する
double value=0.0;
for(int i=begin; i<end; ++i){
    output[i] = f(input[i])
}
```
現状の`RcppParallel(4.3.15)` では `parallelFor()` `parallelReduce()` は DataFrame のカラムや List の要素毎の並列化には対応していない。これを行うには TBB の機能用いて実装する必要がある。


### RVector, RMatrix

マルチスレッド処理では、入力データや出力データの同じ要素に対して、異なる並列スレッドが同時にアクセスすることを防ぐ "スレッドセーフ" なデータアクセスが必要がある。

`RcppParallel` では Rcppの `Vector` や　`Matrix` に対してスレッドセーフにアクセスするためのラッパー `RVector` `RMatrix`を提供している。



```cpp
//整数ベクターを RVector<int> に変換する。
IntegerVector v_int;
RVector<int> vp_int(v_int);

//実数行列を Rmatrix<double> に変換する。
NumericMatrix m_num;
Rmatrix<double> mp_num(m_num);

```

## Worker

`parallelFor` `parallelReduce` で処理する内容は関数オブジェクトとして定義する。

`parallelFor``parallelReduce` に渡す関数オブジェクトは `Worker` クラスを継承して作成する。


## 例：parallelFor()

`parallelFor` を使って、`Matrix` の各要素の平方根を計算する。
http://gallery.rcpp.org/articles/parallel-matrix-transform/


``` cpp
// [[Rcpp::depends(RcppParallel)]]
#include <RcppParallel.h>
using namespace RcppParallel;

// Worker を継承して関数オブジェクト SquareRoot を定義する
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
  SquareRoot my_sqrt(x, output);
  
  // parallelFor()を使って、
  // 入力データの全ての要素に対して関数オブジェクトを適用する
  // この中で output に値がセットされる
  parallelFor(0, x.length(), my_sqrt);
  
  // エラー：この記述は誤り parallelFor() の返値は void
  // output = parallelFor(0, x.length(), squareRoot);
  
  // 結果を出力
  return output;
}
```


上の Rcpp コード例

```r
#関数オブジェクト SquareRoot の operator()の定義
SquareRoot <- function( begin, end){
   #関数の外の output_data を書き換える
   output_data[begin:end] <<- sqrt(input_data[begin:end])
   invisible(NULL)
}

#関数オブジェクトをインスタンス化するときに
#入力データと出力データを初期化される
input_data  <- c(1)
output_data <- rep(NA, 10)

# parallelFor()を実行するときに引数として指定される
begin <- 1
end <- 10

# parallelFor() は i を幾つかに分割して
# SquareRoot() を並列で実行する
i <- begin:end
SquareRoot(i[1], i[3])
SquareRoot(i[4], i[6])
SquareRoot(i[7], i[10])

#処理後の値を表示する
print(input_data)
print(output_data)

```

## 例：parallelReduce()

ベクターの要素の合計値を計算する
http://gallery.rcpp.org/articles/parallel-vector-sum/

```cpp
// [[Rcpp::depends(RcppParallel)]]
#include <RcppParallel.h>
using namespace RcppParallel;

struct Sum : public Worker
{   
  // 入力値
  const RVector<double> input_data;
  
  // 合計値
  // この変数の型は RVector<T> の要素の型 T と一致している必要がある
  double value;
  
  //最初に入力データを取得するためのコンストラクタ
  Sum(const NumericVector input) : input_data(input), value(0) {}
  //分割された入力データ(sum.input_data)を受け取って、スレッドに渡すときに使われるコンストラクタ
  Sum(const Sum& sum, Split) : input_data(sum.input_data), value(0) {} 
  
  
  // input_data の要素番号 begin から要素番号 (end - 1) までの要素の合計値を計算する 
  void operator()(std::size_t begin, std::size_t end) {
    value += std::accumulate(input_data.begin() + begin, input_data.begin() + end, 0.0);
  }
  
  // 他のスレッドで計算された結果を、このスレッドで計算された結果と、合体させるための処理
  void join(const Sum& rhs) { 
    value += rhs.value; 
  }
};



// [[Rcpp::export]]
double parallelVectorSum(NumericVector x) {
  
  // 入力データ x を渡して関数オブジェクトをインスタンス化
  Sum sum(x);
  
  // 要素番号 0 から x.length() -1 までの要素の合計を求める
  parallelReduce(0, x.length(), sum);
  
  // 合計値を返す
  return sum.value;
}

```




##パッケージで利用する場合

各ファイルに以下の記述を追加する

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
