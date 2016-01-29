# RcppParallel

公式サイト：http://rcppcore.github.io/RcppParallel/


RcppParallel Rcpp で並列プログラミングを記述することを支援するパッケージ。バックエンドとして Windows, OS X, Linux では Intel Threaded Building Blocks (TBB) ライブラリ、その他のプラットフォームでは TinyThread ライブラリを用いている。

####他のRの並列計算パッケージとの違い

Rには他にも parallel や snow などの並列化パッケージが存在するが、これらとの違いはなんだろうか。

parallel や snow での並列化は **マルチプロセス** である。複数のRを別プロセスとして立ち上げて並列で実行する。そのため元のRから並列計算を行うRにデータを転送する必要がある。１台のPCで並列計算を行う際にも並列プロセス間で　socket 通信を介してデータをコピーするため、データが大きい場合には非常に時間がかかってしまう。

一方、RcppParallelでの並列化は **マルチスレッド** である。そのため、１台のコンピュータの複数コアでのみ並列計算を行うことができる。しかし、並列スレッドは元のRのメモリ上のデータを共有できるため、データ転送のコストがかからない。


マルチスレッドのプログラミングは技術的ハードルがやや高く初心者には難しいものであった。しかし、RcppParallelを使うと、スレッド並列化に必要な処理
を内部で行ってくれるので、ユーザーが記述するべきコードはシンプルなもので済む。


インストール

```
install.packages("RcppParallel")
install_github("RcppParallel","RcppCore")
```

Rcppソースに以下を追加
```
// [[Rcpp::depends(RcppParallel)]]
#include <RcppParallel.h>
```

##パッケージで利用する場合


**DESCRIPTION**

```
Imports: RcppParallel
LinkingTo: RcppParallel
SystemRequirements: GNU make
```
N

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