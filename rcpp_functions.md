#Rcpp が提供する R ライクな関数

ベクターがNAを含まないことを保証できるなら、以下のようにすると計算が早くなる
noNA(x) ;

mean(x);



Rcpp

ベクター関係

```
head(), tail(), rep_each(), rep_len(), rep(), rev()
seq_along(), seq_len(), seq(), cumsum(), diff(), pmin(), and pmax()
```

数学関数

```
abs(), acos(), asin(), atan(), beta(), ceil(), ceiling(), choose(),cos(), cosh(), 
digamma(), exp(), expm1(), factorial(), floor(), gamma(), lbeta(),lchoose(),
lfactorial(), lgamma(), log(), log10(), log1p(), pentagamma(), psigamma(),
round(), signif(), sin(), sinh(), sqrt(), tan(), tanh(), tetragamma(), trigamma(),trunc().
```


要約統計量

```
mean(), min(), max(), sum(), sd(), var()
```


値の検索

```
match(), self_match(), which_max(), which_min()
```


重複の値

```
duplicated(), unique()
```


確率分布
d/q/p/r 全てのRの提供する確率分布




###seq


###seq_along

IntegerVector x = IntegerVector::create( 0, 1, NA_INTEGER, 3 ) ;
seq_along( x ) // 1:length(x)

###seq_len

NumericVector x(10);
x = seq_len(10); //1:10







###rep_len

NumericVector x1 = rep_len(x, n);
all, any


bool res = is_true( any( x < y ) )
bool res = is_false( any( x < y ) )
bool res = is_na( any( x < y ) )


###NA

// [[Rcpp::export]]
IntegerVector timesTwo() {
  IntegerVector y = {1,NA_INTEGER,3}; //C++11 イニシャライザリスト
  //IntegerVector::create( 1, NA_INTEGER, 3 ) ; //通常版
    //y[1] C++ では変な値が入っているので注意
   return 2*y;
}

> timesTwo()
[1]  2 NA  6


###pmin, pmax


NumericVector z = pmin(x, y)  //x[i] と y[i] を比較して小さい方を z[i] とする
NumericVector z = pmax(x, y)  //x[i] と y[i] を比較して大きい方を z[i] とする
ifelse

x, y, z, w はベクター
ifelse(x > y, z, w);   //(x>y)[i] が真なら、z[i], 偽なら w[i] 
ifelse(x > y, 1, w);   //(x>y)[i] が真なら、1, 偽なら w[i] 


###sapply

第１引数の各要素に、第２引数で与えた関数を適用した、結果をベクターで返す

//テンプレート関数を渡す場合
template <typename T>
T square( const T& x){
    return x * x ;
}
sapply( seq_len(10), square<int> ) ;

//関数オブジェクトを与える場合
template <typename T>
struct square : std::unary_function<T,T> {
    T operator()(const T& x){
        return x * x ;
    }
}
sapply( seq_len(10), square<int>() ) ;

//std::function と ラムダ式で書く場合
 std::function<int (int)> func_obj = [](int x) { return (x*x);};
 sapply( seq_len(10), func_obj) ;


###lapply

sapplyと似ているが、返り値がlistになっている。

###sign

数値ベクターを受け取り、その各要素の符号に応じて、-1,0,1の値を格納したベクターを返す。

> sign(-3:3)
[1] -1 -1 -1  0  1  1  1


###diff

与えた数値ベクターの隣り合う要素の差を格納したベクターを返す。
y = diff(x) // y[i] == x[i+1] - x[i]

> diff(c(1,4,6))
[1] 3 2
数学関数


abs(x) //絶対値
exp(x) //eのx乗
floor(x) //xの切り下げ
ceil(x) //xの切り上げ
pow(x, 2)  //xの2乗



```



