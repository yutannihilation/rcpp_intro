# コンソールへの出力

`Rprintf()` `Rcout` はRの画面にオブジェクトの値やメッセージを出力する。`Rcerr`はエラー出力の際に使う。

```
Rprintf(format, ... );
Rcout << "message" << "\n";
Rcerr << "error message" << "\n";
```

```
template <int MAX_SIZE>
std::string sprintf( const char *format, ...)
```
