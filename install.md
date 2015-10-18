# インストール

#開発ツールのインストール

まずは C++ のコンパイラをインストールしなければ始まりません。

Windows では [Rtools](https://cran.r-project.org/bin/windows/Rtools/index.html) をインストールします。

C:\\Rtools 以下にgccなどがインストールされる。

Mac なら Xcode をインストール

Linux なら
sudo apt-get install r-base-dev texlive-full

筆者の環境
* Windows 7 64bit
* Mac OS X 10.9.2　Xcode 5.1.1
* R 3.1.0
* RStudio 0.98.507
* Rtools 3.1
* Rcpp 0.11.1

必要に応じて（自分でインストールした gcc を使いたい場合など）、以下のファイルで環境変数を設定する。

```
$HOME/.R/Makevars
CC=/opt/local/bin/gcc-mp-4.7
CXX=/opt/local/bin/g++-mp-4.7
CPLUS_INCLUDE_PATH=$CPLUS_INCLUDE_PATH:/opt/local/include
LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/local/lib
CXXFLAGS= -g0 -O3 -Wall
MAKE=make -j4
```




#Rcpp のインストール 

Rで

```r
install.packages("Rcpp")
library(Rcpp)
```