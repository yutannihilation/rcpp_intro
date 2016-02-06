# コンソールへの出力

`Rprintf()` `Rcout` を使うと、Rの画面にメッセージやオブジェクトの値を出力することができる。`Rcerr`はエラー出力の際に使う。

```
Rprintf(format, ... );
Rcout << "message" << "\n";
Rcerr << "error message" << "\n";
```

```
template <int MAX_SIZE>
std::string sprintf( const char *format, ...)
```
