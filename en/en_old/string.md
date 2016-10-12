# String

`String` は、`CharacterVector`, `CharacterMatrix` の要素となるスカラー型である。

##作成

```cpp
String s;      //""
String s("X"); //"X"

//str の値をコピーする
String s(str);          
String s(str, "UTF-8"); //encodingも指定する

//長さ１の文字列ベクター char_vec の値をコピーする
String s(char_vec) 
String s(char_vec, "UTF-8") ////encodingも指定する
```

##演算

`String` には　`+=` 演算子が定義されている。文字列の後ろに、別の文字列を付加できる。 （`+`演算子は定義されていない）

```
String s("A");
s += "B";
//s = s + "B" //コンパイルエラー
Rcout << s << endl; //"AB"
```

## メソッド

#### replace_first( str, new_str)

この String オブジェクトの最初に見つけた文字列 str と一致する部分を 文字列 new_str に置き換える。

単に置き換えた文字列を返すだけはなく、このオブジェクトの値を書き換えてしまうことに注意する。（このページの最後のコード例を参照）

#### replace_last( str, new_str) 

この Stringオブジェクトの、最後に見つけた部分文字列 str を 文字列 new_str に置き換える。

#### replace_all( str, new_str) 

この String オブジェクトから文字列 str と一致する部分文字列を全て、文字列 new_str に置き換える。

#### push_back(str)

この String オブジェクトの末尾に文字列 str を追加する。


#### push_back(str)
strをsの先頭に追加

#### set_na()
NAにする

#### get_cstring()

#### get_encoding()

#### set_encoding("X")

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
