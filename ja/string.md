# String

`String` は、`CharacterVector` の要素に対応するスカラー型です。`String` は C の文字列 `char*` や C++ の文字列 `std::string` では対応していない NA 値（`NA_STRING`）も扱うことができます。

## String オブジェクトの作成

`String` 型のオブジェクトの作成方法には、大別して、C/C++の文字列から作成する方法、別の `String` オブジェクトから作成する方法、長さが文字列ベクトルの１つの要素から作成する方法、の3通りの方法があります。また、文字コードも合わせて指定することができます。

```cpp
//文字列を指定する
String s;      // ""
String s("X"); // "X"

//String 型の文字列 str の値をコピーして作成する
String s(str);          
String s(str, "UTF-8"); //encodingも指定する

//文字列ベクトル char_vec の１つの要素の値をコピーして作成する
String s(char_vec[0])
String s(char_vec[0], "UTF-8") //encodingを指定する
```

## 演算子

`String` には　`+=` 演算子が定義されています。これにより文字列の末尾に別の文字列を結合できます。 （ `+` 演算子は定義されてないので注意してください）

```
// String オブジェクトの作成
String s("A");

// 文字列を結合します
s += "B";

Rcout << s << "\n"; //"AB"
```


## メンバ関数

注：メンバ関数 `replace_first()`, `replace_last()`, `replace_all()` は単に文字を置き換えた文字列を返すわけではなく、このオブジェクトの値をそのものを書き換えます。

#### replace_first( str, new_str)

この String オブジェクトの中で文字列 str と一致する最初に見つけた部分文字列を文字列 new_str に置き換えます。


#### replace_last( str, new_str) 

この String オブジェクトの中で、文字列 str と一致する最後に見つけた部分文字列 str を 文字列 new_str に置き換えます。

#### replace_all( str, new_str) 

この String オブジェクトの中で、文字列 str と一致する全ての部分文字列 str を 文字列 new_str に置き換えます。

#### push_back(str)

この String オブジェクトの末尾に文字列 str を結合します。（ += 演算子と同じ機能）


#### push_back(str)

この String オブジェクトの先頭に文字列 str を結合します。

#### set_na()

NA 値をセットします。

#### get_cstring()

String オブジェクトの文字列を C 言語の文字列定数（const char*）に変換して返します。

#### get_encoding()

文字コード（"bytes", "latin1", "UTF-8", "unknown"のいずれか）を返します。

#### set_encoding(str)

文字列 str で指定する文字コード設定します。




### コード例

```
// [[Rcpp::export]]

void rcpp_string(){

  String s("abcdabcd");
  
  Rcout << s.replace_first("ab", "AB").get_cstring() << endl;
  Rcout << s.get_cstring() << endl;
  
  s="abcdabcd";
  Rcout << s.replace_last("ab", "AB").get_cstring()  << endl;
  
  s="abcdabcd";
  Rcout << s.replace_all("ab", "AB").get_cstring()  << endl;
}
```
###実行結果
```
> rcpp_string()
ABcdabcd
ABcdabcd
abcdaABd
ABcdABcd

```
