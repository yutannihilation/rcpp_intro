#R ライクな関数

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
[diff()](#diff)
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



#ベクター関係

####head()
####tail()
####rev()
####rep()
####rep_each()
####rep_len()
NumericVector x1 = rep_len(x, n);
####seq()
####seq_along()
####seq_len()

```
IntegerVector x = IntegerVector::create( 0, 1, NA_INTEGER, 3 ) ;
seq_along( x ) // 1:length(x)

NumericVector x(10);
x = seq_len(10); //1:10
```


## 文字列

#### collapse()


## 値の検索

####match()
####self_match()
####which_max()
####which_min()


## 重複の値

####duplicated()
####unique()
####sort_unique()

## 集合
####setdiff()
####setequal()
####intersect()
####union_()

#最大値・最小値
####min()
####max()
####cummax()
####cummin()
####pmin()
####pmax()

NumericVector z = pmin(x, y)  //x[i] と y[i] を比較して小さい方を z[i] とする
NumericVector z = pmax(x, y)  //x[i] と y[i] を比較して大きい方を z[i] とする

####range()
####clamp()


数学

####sum()
####mean()
####sd()
####var()
####cumsum()
####cumprod()
####floor()
####ceil()
####ceiling()
####round()
####trunc()
####diff()
####sign()
数値ベクターを受け取り、その各要素の符号に応じて、-1,0,1の値を格納したベクターを返す。

> sign(-3:3)
[1] -1 -1 -1  0  1  1  1
####abs()
####pow()
####sqrt()
####exp()
####expm1()
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


###pmin, pmax



ifelse




```






###diff

与えた数値ベクターの隣り合う要素の差を格納したベクターを返す。

```
y = diff(x) // y[i] == x[i+1] - x[i]
```

```
> diff(c(1,4,6))
[1] 3 2
```

数学関数


abs(x) //絶対値
exp(x) //eのx乗
floor(x) //xの切り下げ
ceil(x) //xの切り上げ
pow(x, 2)  //xの2乗



```



