# Good practices




## 

If you want to use compiler installed under non stantard place
自分でインストールした gcc や clang を使いたい場合など、必要に応じて、ユーザーのホームディレクトリに次のファイルに以下のファイルを作成し、そこに環境変数の設定を記述する。


* `.R/Makevars` Linux, Mac
* `.R/Makevars.win` Windows

```
CC=/opt/local/bin/gcc-mp-4.7
CXX=/opt/local/bin/g++-mp-4.7
CPLUS_INCLUDE_PATH=/opt/local/include:$CPLUS_INCLUDE_PATH
LD_LIBRARY_PATH=/opt/local/lib:$LD_LIBRARY_PATH
```

```
CXXFLAGS= -g0 -Wall
MAKE=make -j4
````



ユーザーのホームディレクトリは次のコードで調べることができる。
```
path.expand("~")
```