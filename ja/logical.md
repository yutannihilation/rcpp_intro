# 論理演算

LogicalVector の要素ごとの論理演算には演算子 `&`（論理積） `|`（論理和） `!`（論理否定）を用いる。

```
LogicalVector v1 = {1,1,0,0}; // int 型で初期化
LogicalVector v2 = {true,false,true,false}; // bool 型で初期化

```






```R
LogicalVector v1 = {0,1,-1,2}; // int 型で初期化
LogicalVector v2 = {true,false,true,false}; // bool

Rcout 

LogicalVector res1 = 

```

C++ では論理値は `bool` 型であるので、LogicalVector の要素の型は bool であると思うかもしれないが、実際には int 型である。つまり、これは R の論理ベクトルの実体は整数ベクトルであることを意味する。 なので、論理ベクトルの要素は bool ではなく int である。なぜかというと R の論理ベクトルの要素の値には TRUE, FALSE だけではなく NA もあるが、bool で表現できるのは true と false だけであるためだ。 




（標準C++ の bool 型の論理演算は `&&` と `||` であるのがややこしい）

R では論理ベクトルの実体は整数ベクトルである。 LogicalVector の実体は Integer

１つ１つの要素ごとにTRUE

R では論理ベクトルの要素の値は TRUE(1), FALSE(0), NA(intの最大値) があり得る。（R では論理ベクトルの実体は整数ベクトルなので、TRUE は1, FALSE は 0, NA は int の最大値で表現されている）
しかし、C++ の bool 型の値は true, false のみなので NA を扱うことができない。





LogicalVector v




[all()](#all)
[any()](#any)
[ifelse()](#ifelse)
[is_finite()](#isfinite)
[is_infinite()](#isinfinite)
[is_na()](#isna)
[is_nan()](#isnan)
[is_true()](#istrue)
[is_false()](#isfalse)

####all()

all(v)

論理ベクター v を受け取り、全ての要素が TRUE の時、TRUEを返す。

####any()

any(v)

論理ベクター v を受け取り、いずれかのの要素が TRUE の時、TRUEを返す。


```cpp
NumericVector v1 = NumericVector::create(1,2,3);
NumericVector v2 = NumericVector::create(2,1,3);

LogicalVector lv1 = all( v1 > v2 ); //FALSE
LogicalVector lv2 = any( v1 > v2 ); //TRUE
```

`all()`, `any()` の結果を bool 型に変換する場合には、`is_true()` `is_false()` `bool()`を使う。

```cpp
// bool 型への変換
bool b1 = is_true(all( v1 > v2 ));  //false
bool b2 = is_false(all( v1 > v2 )); //true
bool b3 = bool(all( v1 > v2 ));     //false
```


####ifelse()

ifelse(cond, x1, x2)

論理ベクター cond を受け取り、TRUEの時には x1, FALSE の時には x2 を返す。x1, x2 はベクターでもスカラーでも良いが、ベクターの長さは cond と一致している必要がある。

```cpp
// [[Rcpp::export]]
void rcpp_ifelse(){
  NumericVector   v1  = NumericVector::create(1,2,3,4,5);
  NumericVector   v2  = NumericVector::create(2,1,4,3,5);
  
  NumericVector res1 = ifelse(v1 > v2, 0, v1);
  NumericVector res2 = ifelse(v1 > v2, v1, 0);
  NumericVector res3 = ifelse(v1 > v2, v1, v2);
  
  Rcout << "ifelse(v1 > v2, 0, v1) " << res1 << endl;
  Rcout << "ifelse(v1 > v2, v1, 0) " << res2 << endl;
  Rcout << "ifelse(v1 > v2, v1, v2) " << res3 << endl;
}
```