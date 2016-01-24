# コンソールへの出力

`Rprintf()` `Rcout` はRの画面にオブジェクトの値やメッセージを出力する。`Rcerr`はエラー出力の際に使う。

```
Rprintf(format, value);
Rcout << "message" << "\n";
Rcerr << "error message" << "\n";
```