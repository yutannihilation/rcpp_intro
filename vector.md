# ベクトル


* `IntegerVector`
* `NumericVector`
* `ComplexVector`
* `CharacterVector` (`StringVector`)
* `LogicalVector`
* `DateVector`
* `DatetimeVector`
* `RawVector`




###作成

```
NumericVector v(3); // c(0,0,0)と同義
NumericVector v = NumericVector::create(1,2,3); //c(1,2,3) 
NumericVector v = NumericVector::create(Named("x") = 1 , _["y"] = 2); //c(x=1, y=2) 名前付きベクター
NumericVector v = {1,2,3}; //c(1,2,3)、C++11イニシャライザリストを使った例
```



###要素へのアクセス

Rと同様に、Rcpp数値ベクター・文字列ベクター・論理ベクターを使って、ベクター要素にアクセスして、値を参照・代入することができる。

```
v[0]       //要素番号（数値ベクター）でアクセス
v["X"]     //要素の名前（文字列ベクター）でアクセス
v[L]       //論理値ベクター(L)でアクセス
```

**重要**：
Rcppでは、ベクターや行列の要素のインデックスはC++のスタイルに従っているため０から始まるので注意すること。


```
//例：ベクターvの総和を計算する
double sum=0;
for(int i=0; i<v.length(); ++i){
    sum += v[i];
}
```


## ベクター同士の代入
**重要**：ベクターや行列同士で代入する際には注意が必要である。単純に `v2 = v1;`で代入すると v1 の値が v2 にコピーされるのではなく、v1 と v2 は同じオブジェクトに対する別名となるので、v1の値を変更するとv2の値も変更される。

```
NumericVector v1(1,2,3);
NumericVector v2 = v1; //単純な代入

v1[0] = 100; //v1を変更する

//値の確認
//v1への変更が、v2にも影響する
Rcout << v1 << endl; //100 2 3
Rcout << v2 << endl; //100 2 3
```
値をコピーしたい場合には `clone()` 関数を使用する。そうすると v1 の値を変更しても、v2 の値は変更されない。

```
NumericVector v1(1,2,3);
NumericVector v2 = clone(v1);
v1[0] = 100;
Rcout << v1 << endl; //100 2 3
Rcout << v2 << endl; //1 2 3
```

(C++に詳しい人のために説明すると、Rcppのデータ型は内部にオブジェクトの値そのものではなく、オブジェクトへのポインタを保持している。そのため、`v2=v1` とするとポインターの値がコピーされてしまう）



#メソッド

static stored_type Vector::get_na()
static bool Vector::is_na()
static Vector create()


R_xlen_tはスカラー

R_xlen_t length()
R_xlen_t size() //length()と同じ
R_xlen_t offset(const int& i, const int& j)
R_xlen_t offset(const R_xlen_t& i)
R_xlen_t offset(const std::string& name)

void fill( const U& u)

iterator begin()
iterator end()

アクセス
v["名前"]
v("名前")
v(int i)
v.at(int i)
v(1)
v(1,1)

Vector& sort()

void assign( InputIterator first, InputIterator last)
このベクターにfirst, last で指定された値を代入する？

Vector import( InputIterator first, InputIterator last)
first, last で指定された値が代入されたベクターを返す

Vector import_transform( InputIterator first, InputIterator last, F f)
first, last で指定された値、を 関数 f で変換した値、が代入されたベクターを返す

void push_back( const T& object)
void push_back( const T& object, const std::string& name )

void push_front( const T& object)
void push_front( const T& object, const std::string& name)


iterator insert( iterator position, const T& object)
iterator insert( int position, const T& object)

iterator erase( int position)
iterator erase( iterator position)
iterator erase( int first, int last){
iterator erase( iterator first, iterator last)

void update(SEXP)　?

static void replace_element( iterator it, SEXP names, R_xlen_t index, const U& u)

bool containsElementNamed( const char* target )
このベクターが指定された名前の要素を持っているか

int findName(const std::string& name)
指定された名前の（最初の）要素のインデックスを返す。見つからなかったら -1


SEXP eval()
グローバル環境でこのベクターを評価する？

SEXP eval(SEXP env)
環境 env でこのベクターを評価する？