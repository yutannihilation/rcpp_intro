#R ライクな関数

Rに存在する関数と類似した機能もつRcppの関数の一覧を示す。名前と機能がRの関数と一致しているものについては、説明を省略しているので、Rのヘルプを参照して欲しい。

これらの関数に与えるオブジェクトにNAが含まれていないことが保証できる場合には、`noNA()`を使ってオブジェクトに印をつけると、Rcpp関数がNAのチェックを行わなくなるので、計算が早くなる。

```
NumericVector v = NumericVector::create(1,2,3,4,5);
NumericVector res = mean(noNA(v));
```


**ベクター関係**

[head()](#head)
[tail()](#tail)
[rev()](#rev)
[rep()](#rep)
[rep_each()](#rep_each)
[rep_len()](#rep_len)
[seq()](#seq)
[seq_along()](#seq_along)
[seq_len()](#seq_len)
[diff()](#diff)

**文字列**

[collapse()](#collapse)


**値の検索**

[match()](#match)
[self_match()](#self_match)
[which_max()](#which_max)
[which_min()](#which_min)


**重複の値**

[duplicated()](#duplicated)
[unique()](#unique)
[sort_unique()](#sort_unique)

**集合**

[setdiff()](#setdiff)
[setequal()](#setequal)
[intersect()](#intersect)
[union_()](#union_)

**最大値・最小値**

[min()](#min)
[max()](#max)
[cummax()](#cummax)
[cummin()](#cummin)
[pmin()](#pmin)
[pmax()](#pmax)
[range()](#range)
[clamp()](#clamp)

**数学**

[sum()](#sum)
[mean()](#mean)
[sd()](#sd)
[var()](#var)
[cumsum()](#cumsum)
[cumprod()](#cumprod)


[floor()](#floor)
[ceil()](#ceil)
[ceiling()](#ceiling)
[round()](#round)
[trunc()](#trunc)



[sign()](#sign)
[abs()](#abs)
[pow()](#pow)
[sqrt()](#sqrt)
[exp()](#exp)
[expm1()](#expm1)
[log()](#log)
[log10()](#log10)
[log1p()](#log1p)
[sin()](#sin)
[sinh()](#sinh)
[cos()](#cos)
[cosh()](#cosh)
[tan()](#tan)
[tanh()](#tanh)
[acos()](#acos)
[asin()](#asin)
[atan()](#atan)

[gamma()](#gamma)
[lgamma()](#lgamma)
[digamma()](#digamma)
[trigamma()](#trigamma)
[tetragamma()](#tetragamma)
[pentagamma()](#pentagamma)
[psigamma()](#psigamma)
[factorial()](#factorial)
[lfactorial()](#lfactorial)
[choose()](#choose)
[lchoose()](#lchoose)
[beta()](#beta)
[lbeta()](#lbeta)


**論理**

[all()](#all)
[any()](#any)
[ifelse()](#ifelse)
[is_finite()](#is_finite)
[is_infinite()](#is_infinite)
[is_na()](#is_na)
[is_nan()](#is_nan)
[is_true()](#is_true)
[is_false()](#is_false)

**NA**

[na_omit()](#na_omit)



**Apply関数**

[lapply()](#lapply)
[sapply()](#sapply)
[mapply()](#mapply)




**確率分布**

全てのRの提供する確率分布

d/q/p/r 

[dpqr関数の章を参照](dpqr_functions.md)

**その他**

[table()](#table)

----



#ベクター関係

####head()

head(v, n)

####tail()

tail(v, n)

####rev()

rev(v)

####rep()

rep(x, n)

####rep_each()

rep_each(v, times)

R の rep.int(v, times) と同義

```
// [[Rcpp::export]]
NumericVector rcpp_rep_each(){
  NumericVector v = NumericVector::create(1,2,3);
  return rep_each(v, 3);
}

// 1 1 1 2 2 2 3 3 3
```

####rep_len()

rep_len(v, n);

```
// [[Rcpp::export]]
NumericVector rcpp_rep_len(){
  NumericVector v = NumericVector::create(1,2,3);
  return rep_len(v, 10);
}
//1 2 3 1 2 3 1 2 3 1
```


####seq()

seq(start, end)

start から end までの連続し整数の数列を返す。


```
IntegerVector v = seq(-3,3);

```
####seq_along()

seq_along(v)

####seq_len()

seq_len(int n)

####diff()

diff(v)




## 文字列

#### collapse()

collapse(v)

文字列ベクター v の要素を結合した文字列 `String` を返す。

```
// [[Rcpp::export]]
CharacterVector rcpp_collapse(){
  CharacterVector v = CharacterVector::create("A","B","C");
  String s = collapse(v);
  return wrap(s);
} //"ABC"
```




## 値の検索

####match()

match(v, table)

ベクター table の要素と値が一致する、ベクター v の要素の位置に table の要素番号を入れたベクター返す。

```
// [[Rcpp::export]]
IntegerVector rcpp_match(){
  CharacterVector v     = CharacterVector::create("AB","A","B","B","C","D","E");
  CharacterVector table = CharacterVector::create("B","D");
  IntegerVector m = match(v, table);
  return m;
}
//NA NA  1  1 NA  2 NA
```

####self_match()

self_match(v)

match(v, table) の v と table に同じベクターを渡したのと同義

```
// [[Rcpp::export]]
IntegerVector rcpp_self_match(){
  CharacterVector v     = CharacterVector::create("A","A","B","B","C","D","E");
  return self_match(v);
}
//1 1 2 2 3 4 5
```


####which_max()

which_max(v)

R の which.max()と同義

####which_min()

which_max(v)

## 重複の値

####duplicated()

duplicated(v)

####unique()

unique(v)

####sort_unique()

sort_unique(v)

unique()の結果をソートしたベクターを返す。

```
// [[Rcpp::export]]
void rcpp_sort_unique(){
  CharacterVector v     = CharacterVector::create("B", "A","B","E", "C","A","D");
  Rcout << unique(v) << endl;      //"E" "C" "B" "D" "A"
  Rcout << sort_unique(v) << endl; //"A" "B" "C" "D" "E"
}
```

## 集合

####setdiff()

setdiff( v1, v2);

####setequal()

setequal( v1, v2);

####intersect()

intersect( v1, v2);

####union_()

union_( v1, v2);

#最大値・最小値

####min()

min(v)

####max()

max(v)

min(), max() を`CharacterVector` に対して適用することはできない。

```
// [[Rcpp::export]]
void rcpp_min(){
  NumericVector   num_v  = NumericVector::create(1, 2, 3, 4, 5);
  CharacterVector char_v = CharacterVector::create("A","B","C","D","E");
  
  NumericVector num_v1(2);
  num_v1[0] = min(num_v);
  num_v1[1] = max(num_v);
  
  CharacterVector char_v1(2);
  char_v1[0] = min(char_v);
  char_v1[1] = max(char_v);
  
  Rcout << num_v1 << endl;  // 1 5
  Rcout << char_v1 << endl; // "D" "B"
}
```


####cummin()

cummin(v)

####cummax()

cummax(v)

####pmin()

pmin(v1,v2)

####pmax()

pmax(v1,v2)


####range()

range(v)

```
NumericVector v = NumericVector::create(1,2,3,4,5);
NumericVector res = range(v);
Rcout << res << endl; // 1 5
```

####clamp()

clamp(min, v, max)

ベクター v の要素に対して、min 未満の値を min に、max 超の値を max に置き換えたベクターを返す。


```
// [[Rcpp::export]]
void rcpp_clamp(){
  NumericVector   v  = NumericVector::create(1,2,3,4,5);
  NumericVector res = clamp(2, v, 3);
  Rcout << res << endl;
}
```


##数学

####sum()

sum(v)

####mean()

mean(v)

####sd()

sd(v)

####var()

var(v)

####cumsum()

cumsum(v)

####cumprod()

cumprod(v)

####floor()

flood(v)

####ceil()

ceil(v)

ceiling() と同義

####ceiling()

ceiling(v)

####round()

round(v, digits)

####trunc()

trunc(v)

```
// [[Rcpp::export]]
  void rcpp_round(){
    NumericVector   v  = rnorm(10, 0, 10);
    Rcout <<"v "<< v << endl;
    Rcout << "ceil(v) "<< NumericVector(ceil(v)) << endl;
    Rcout << "floor(v) "<< NumericVector(floor(v)) << endl;
    Rcout << "round(v, 0) "<< NumericVector(round(v, 0)) << endl;
    Rcout << "round(v, 1) "<< NumericVector(round(v, 1)) << endl;
    Rcout << "trunc(v) "<< NumericVector(trunc(v)) << endl;
  }
```
```
> rcpp_round()
v -1.5216 -2.47017 9.47903 12.0625 14.7051 -7.40828 15.5769 -8.93159 -3.68999 -8.25377
ceil(v) -1 -2 10 13 15 -7 16 -8 -3 -8
floor(v) -2 -3 9 12 14 -8 15 -9 -4 -9
round(v, 0) -2 -2 9 12 15 -7 16 -9 -4 -8
round(v, 1) -1.5 -2.5 9.5 12.1 14.7 -7.4 15.6 -8.9 -3.7 -8.3
trunc(v) -1 -2 9 12 14 -7 15 -8 -3 -8
```

####sign()

sign(v)

####abs()

abs(v)

####pow()

pow(v, n)

ベクター v の各要素を n 乗したベクターを返す。
Rの `v^n` と同義’


####sqrt()
####exp()

####expm1()

expm1(v)

`exp(v) -1` と同義

####log()
####log10()
####log1p()
####sin()
####sinh()
####cos()
####cosh()
####tan()
####tanh()
####acos()
####asin()
####atan()
####gamma()
####lgamma()
####digamma()
####trigamma()
####tetragamma()
####pentagamma()
####psigamma()
####factorial()
####lfactorial()
####choose()
####lchoose()
####beta()
####lbeta()

##論理

####all()
####any()
####ifelse()
####is_finite()
####is_infinite()
####is_na()
####is_nan()
####is_true()
####is_false()

## NA
####na_omit()



## apply 関数

####lapply()
sapplyと似ているが、返り値がlistになっている。

```
template <typename T>
T square( const T& x){
	return x * x ;
}

// [[Rcpp::export]]
List apply_square( NumericVector x ){
	return lapply( x, square<double> ) ;
}
```
####sapply()

第１引数の各要素に、第２引数で与えた関数を適用した、結果をベクターで返す

```
//テンプレート関数を渡す場合
template <typename T>
T square( const T& x){
    return x * x ;
}
sapply( seq_len(10), square<int> ) ;
```
```
//関数オブジェクトを与える場合
template <typename T>
struct square : std::unary_function<T,T> {
    T operator()(const T& x){
        return x * x ;
    }
}
sapply( seq_len(10), square<int>() ) ;
```

```
//std::function と ラムダ式で書く場合
 std::function<int (int)> func_obj = [](int x) { return (x*x);};
 sapply( seq_len(10), func_obj) ;
 ```
 
####mapply()




####table()




