# ベクトル

* [作成](#作成)
* [要素へのアクセス](#要素へのアクセス)
* [ベクトル同士の代入における注意点](#ベクトル同士の代入における注意点)
* [メソッド](#メソッド)
* [static メソッド](staticメソッド)


###作成



```
// 長さが 3 で要素の値が 0.0 のベクトル
NumericVector v (3);

// 長さが 3 で要素の値が 1.0 のベクトル
NumericVector v (3, 1.0);

// c(1,2,3) と同等
NumericVector v = NumericVector::create(1,2,3);
NumericVector v = {1,2,3};

// c(x=1, y=2, z=3) と同様 名前付きベクトル
NumericVector v =
  NumericVector::create(Named("x",1), Named("y")=2 , _["z"]=3); 
```

要素名を指定する場合には `Named()` または `_[]`を用いる。
```
Named("name") = value
Named("name", value)
// c(x=1, y=2) 名前付きベクトル
NumericVector v = NumericVector::create(Named("x") = 1 , _["y"] = 2);
```

###要素へのアクセス

Rと同様に、整数（実数）ベクトル・文字列ベクトル・論理ベクトルを使って、ベクトル要素にアクセスして、値を参照・代入することができる。

**【重要】**：
Rcpp は C++ のスタイルに従い、**ベクトルや行列の要素番号は ０ から始まる**ので注意すること。

```
// [[Rcpp::export]]
void access(){
  NumericVector v {10,20,30,40,50};
  v.names() = CharacterVector({"A", "B", "C", "D", "E"});
  
  IntegerVector   i {1,3};
  CharacterVector c {"C","E"};
  
  //要素の値の参照
  NumericVector res1 = v[i];     //整数ベクトルでアクセス
  NumericVector res2 = v[c];     //文字列ベクトルでアクセス
  NumericVector res3 = v[v>=40]; //論理ベクトルでアクセス
  
  Rcout << "res1: " << res1 << "\n";
  Rcout << "res2: " << res2 << "\n";
  Rcout << "res3: " << res3 << "\n";
  
  //ベクトルの一部の要素への値の代入
  v[i] = NumericVector::create(100,200);
  v[c] = NumericVector::create(1000,2000);
  
  Rcout << "v: " << v << "\n";
}


### []演算子の返り値

[]演算子を使ってベクトルのサブセットへアクセスした場合の返値は、正確には `Vector`そのものではなく、`Vector::Proxy` という特殊な型となっている。そのため、`v[]` を、そのまま他の関数に与えるとエラーになることがある。その場合には、`as()` を用いて、目的の `Vector` 型に変換する。as()については


```
NumericVector v {1,2,3,4,5};
IntegerVector i {1,3};
//double x = sum(v[i]); // error
double x = sum(as<NumericVector>(v[i]));
```



## ベクトル同士の代入における注意点
**【重要】**：ベクトルや行列同士で代入する際には注意が必要である。

**単純に `v2 = v1;`で代入すると v1 の値が v2 にコピーされるのではなく、v1 と v2 は同じオブジェクトに対する別名となる**ので、**v1の値を変更するとv2の値も変更される**。

```
NumericVector v1(1,2,3);
NumericVector v2 = v1; //単純な代入

v1[0] = 100; //v1を変更する

//値の確認
//v1への変更が、v2にも影響する
Rcout << v1 << endl; //100 2 3
Rcout << v2 << endl; //100 2 3
```
**元のベクトルの値をコピーしたい場合には `clone()` 関数を使用する**。そうすると v1 の値を変更しても、v2 の値は変更されない。

```
NumericVector v1(1,2,3);
NumericVector v2 = clone(v1);
v1[0] = 100;
Rcout << v1 << endl; //100 2 3
Rcout << v2 << endl; //1 2 3
```

C++に詳しい人のために説明すると、Rcppのデータ型は内部にオブジェクトの値そのものではなく、オブジェクトへのポインタを保持している。そのため、単純に `v2 = v1` で代入するとするとポインターの値がコピーされてしまうので、このような現象が起きる。



##メソッド

メソッドとは、個々のオブジェクトと結びついた関数である。通常の関数とは呼び出し方が少し異なっており、 `v.length()` のような形式で呼び出す。

```
NumericVector v(10);
int n = v.length();
```

#### ベクトルサイズの変更操作の注意

幾つかのメソッドはベクトルのサイズを変更する（`push_back`, `push_front` `erase` `insert`）、しかし、R は C言語で実装されているので、基本的にサイズの変更の効率が良くない。なぜならサイズを変更する際には元のベクトルの値を全てコピーする処理が発生するためだ。必要ならば、`std::vector` など標準C++のコンテナ を使用することを検討すると良いだろう。


####inherits(str)

このオブジェクトの属性 class に文字列 str が含まれているかどうか。

####length()
要素数

####offset(str)
要素名が str である要素のインデックスint

####fill(x)
このベクトルの要素を x（スカラー値） で埋める


####sort()

このベクトルをソートしたベクトルを返す

####assign( first_it, last_it)

イテレーター first_it, last_it で指定された範囲の値を、このベクトル に代入する



####push_back(x)

このベクトル の末尾に x（スカラー） を追加する。

####push_back( x, "x" )
このベクトル の末尾に x（スカラー） を追加する。
追加した要素の名前を "x" とする。


####push_front(x)

このベクトル の先頭に x（スカラー） を追加する。


####push_front( x, "x" )

このベクトル の先頭に x（スカラー） を追加する。
追加した要素の名前を "x" とする。

#### begin()

このベクトルの先頭へのイテレータを返す。

#### end()

このベクトルの末尾へのイテレータを返す。

#### insert( it, x)

このベクトルの it の位置に x を追加し、その要素へのイテレータを返す。


#### erase(i)

このベクトルの i番目の要素を削除し、削除後の同じ位置の要素へのイテレータを返す。

####erase(it)
イテレータ itで指定された要素を削除し、削除後の同じ位置の要素へのイテレータを返す。

####erase(first_i, last_i)

first_i番目 から last_i番目 までの要素を削除し、削除後の同じ位置の要素へのインデックスを返す。


#### erase(first_it, last_it)

first_it から last_it で指定される要素を削除し、削除後の同じ位置の要素へのイテレータを返す。

#### update(v1)

このベクトルの内容を ベクトル v1 と同じにする

#### containsElementNamed(str)

このベクトルが str で指定された名前の要素を持っているかどうかを返す

####findName(str)

指定された名前の（最初の）要素のインデックスを返す。見つからなかったら -1

####eval()
グローバル環境でこのベクトルを評価した結果を返す

####eval(env)
環境 env でこのベクトルを評価した結果を返す



## static メソッド

これらの関数は `NumericVector::create()` のような形式で呼び出す。

####Vector::get_na()

このベクトルの型のNA値を取得する

####Vector::is_na(x)
スカラー値 x がNAであるかどうかを


####Vector::create(x1, x2, ...)
スカラー値 x1, x2, ...  を要素とするベクトルを作成する。指定できる引数の数は20個まで


####Vector::import( first_it, last_it) 
イテレーター first_it, last_it で指定された範囲の値が代入されたベクトルを返す


####Vector::import_transform( first_it, last_it, func)
first, last で指定された範囲の値を、関数 func で変換した値が代入されたベクトルを返す

####replace_element( iterator it, SEXP names, R_xlen_t index, const U& u)

?


