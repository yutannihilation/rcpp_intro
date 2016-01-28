# RcppParallel

公式サイト：http://rcppcore.github.io/RcppParallel/


RcppParallel Rcpp でマルチスレッドの並列プログラミングを記述することを支援するパッケージ。バックエンドとして Windows, OS X, Linux ではIntel the Intel Threaded Building Blocks (TBB)ライブラリ、その他のプラットフォームでは TinyThreadライブラリを用いている。


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

パッケージで利用する場合

`DESCRIPTION` file:

```
Imports: RcppParallel
LinkingTo: RcppParallel
```
`NAMESPACE` file:

```
import(RcppParallel)
```



例

http://gallery.rcpp.org/articles/parallel-matrix-transform/
http://gallery.rcpp.org/articles/parallel-vector-sum/
http://gallery.rcpp.org/articles/parallel-inner-product/
http://gallery.rcpp.org/articles/parallel-distance-matrix/