# インストール

まずは C++ のコンパイラをインストールする。

##開発ツールのインストール

**Windows**：[Rtools](https://cran.r-project.org/bin/windows/Rtools/index.html) をインストールします。C:\\Rtools 以下にgccなどが配置される。

**Mac**：Xcode command line tools をインストールする。ターミナルで次のコマンドを打つ `xcode-select --install`

**Linux**：例えば
`sudo apt-get install r-base-dev texlive-full`



必要に応じて（自分でインストールした gcc を使いたい場合など）、以下のファイルで環境変数を設定する。


$HOME/.R/Makevars

```
CC=/opt/local/bin/gcc-mp-4.7
CXX=/opt/local/bin/g++-mp-4.7
CPLUS_INCLUDE_PATH=$CPLUS_INCLUDE_PATH:/opt/local/include
LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/local/lib
CXXFLAGS= -g0 -O3 -Wall
MAKE=make -j4
```


## Rcpp のインストール 

コンパイラがインストールできたら、R で Rcpp パッケージをインストールする。

```r
install.packages("Rcpp")
```