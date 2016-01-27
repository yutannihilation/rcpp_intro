# パッケージ作成

自作のパッケージでRcppを利用する方法を解説する。

このページのほとんど内容は Hadley Wickham 氏の [R packages のサイト](http://r-pkgs.had.co.nz/src.html)の内容を元にしている。


通常のユーザーにはパッケージを作成する必要はないと考えるかもしれないが、Rcppで書いた関数はパッケージとして保存しておくことを薦める。なぜなら、Rcpp関数をパッケージ化しない場合、Rcppで書いた関数を利用するためには毎回 `sourceCppRcpp` でコンパイルしてRにロードする必要がある。それに対して、パッケージ化では、コンパイル済みのRcpp関数を保存しておき、使うときには`library()` でロードするだけで済むようになるので、時間が節約できる。

##手順

setwd() でパッケージフォルダを作業フォルダとして指定した後に次の関数を実効する。

```
devtools::use_rcpp()
```

* Rcppで書いたソースコードは `src` フォルダの中に配置する
* `DESCRIPTION`ファイルに以下の記述を追加する
```
LinkingTo: Rcpp
Imports: Rcpp
```
* `NAMESPACE`ファイルに以下の記述を追加する
```
useDynLib(mypackage)
importFrom(Rcpp, sourceCpp
```


devtools::use_rcpp()