# パッケージ作成

自作のパッケージでRcppを利用する方法を解説する。

通常のユーザーにはパッケージを作成する必要はないと考えるかもしれないが、Rcppで書いた関数はパッケージとして保存しておくことを薦める。

なぜなら、Rcpp関数をパッケージ化しない場合、Rcppで書いた関数を利用するためには毎回 `sourceCppRcpp` でコンパイルしてRにロードする必要がある。それに対して、パッケージ化しておくことでコンパイル済みのRcpp関数を保存しておき、使うときにはロードするだけで済むようになる



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