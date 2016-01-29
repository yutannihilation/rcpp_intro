# RcppParallel

公式サイト：http://rcppcore.github.io/RcppParallel/


RcppParallel Rcpp でマルチスレッドの並列プログラミングを記述することを支援するパッケージ。バックエンドとして Windows, OS X, Linux ではIntel the Intel Threaded Building Blocks (TBB)ライブラリ、その他のプラットフォームでは TinyThreadライブラリを用いている。

###並列化：マルチスレッドとマルチプロセスの違い

RcppParallelでの並列プログラミングはマルチスレッドなので１台のコンピュータでのみ並列計算を行うことができる。

Rには他にも parallel や snow などの並列化パッケージが存在する。 の１コンピュータでの並列化はマルチプロセス では、並列プロセス間でsocket通信を介してデータをコピーする必要があり、そこに時間がかかる。

マルチスレッドでは、並列スレッド間でメモリ上のデータを共有できるため、データ転送のコストがかからない。その代わり、プログラミング的には注意深く行う必要が有るため、技術的ハードルがやや高い。しかし、RcppParallelでは、マルチスレッド並列化に必要な処理
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