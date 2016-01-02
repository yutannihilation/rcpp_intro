# String

String s; //""
String s(other_String);
String s(other_String, "UTF-8");

String s(ch) //ch要素数１の文字列ベクター
String s(ch, "UTF-8") 

## メソッド

s.replace_first( olds, news) //sから最初に見つけたoldsをnewsに置換
s.replace_last( olds, news)  //sから最後に見つけたoldsをnewsに置換
s.replace_all( olds, news)   //sから全ての見つけたoldsをnewsに置換
s.push_back(str) //strをsの末尾に追加
s.push_back(str) //strをsの先頭に追加
s.set_na() //NAにする

s.get_cstring()
s.get_encoding()
s.set_encoding("UTF-8")

