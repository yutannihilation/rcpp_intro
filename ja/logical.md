# LogicalVector と 論理演算

## LogicalVector の正体

C++ の論理値型は `bool` であるので、`LogicalVector` の要素の型も `bool` であると思うかもしれないが、実際には `int` 型である。(R でも論理ベクトルの実体は整数ベクトルである) なぜこのようになっているかというと `bool` で表現できるのは `true` と `false` だけであるが、R の論理ベクトルの要素の値には `TRUE`, `FALSE` だけではなく `NA` もあり得るためである。

Rcpp では `TRUE` は 1、`FALSE` は 0、`NA` は `NA_LOGICAL`（int の最小値）で表現されている。

|logical|Rcppの記号|int|bool|
|:---:|:---:|:---:|:---:|
|TRUE|TRUE|1 (0以外の値)|true|
|FALSE|FALSE|0|false|
|NA|NA_LOGICAL|int の最小値|true|

## LogicalVector の要素の評価

`LogicalVector` の要素の値を、そのまま if 文の条件式として使用してはならない。なぜなら、C++ の `bool` 型は 0 以外の値を全て `true` と評価するので、`LogicalVector` の `NA`（`NA_LOGICAL`） は `true` と評価されてしまう。

 `LogicalVector` の要素の値を if 文で評価する方法については、次のコード例を参考にして欲しい。

```
// [[Rcpp::export]]
LogicalVector rcpp_logical(){
  
  // NA を含む整数ベクトル
  IntegerVector x = {1,2,3,4,NA_INTEGER};
  
  // 比較演算の結果は LogicalVector となる
  LogicalVector v = (x >= 3); 
  
  // LogicalVector の要素を直接 if 文の条件式で使うと
  // NA_LOGICAL は TRUE と評価されてしまう
  for(int i=0; i<v.size();++i) {
    if(v[i]) Rprintf("v[%i] is evaluated as true.\n",i);
    else Rprintf("v[%i] is evaluated as false.\n",i);
  } 
  
  // LogicalVector の要素の評価する
  for(int i=0; i<v.size();++i) {
    if(v[i]==TRUE) Rprintf("v[%i] is TRUE.\n",i);
    else if (v[i]==FALSE) Rprintf("v[%i] is FALSE.\n",i);
    else if (v[i]==NA_LOGICAL) Rprintf("v[%i] is NA.\n",i);
    else Rcout << "v[" << i << "] is not 1\n";
  }
  
  // TRUE FALSE NA_LOGICAL の値を表示
  Rcout << "TRUE " << TRUE << "\n";
  Rcout << "FALSE " << FALSE << "\n";
  Rcout << "NA_LOGICAL " << NA_LOGICAL << "\n";
  
  return v;
}
```

実行結果

```
> rcpp_logical()
v[0] is evaluated as false.
v[1] is evaluated as false.
v[2] is evaluated as true.
v[3] is evaluated as true.
v[4] is evaluated as true.
v[0] is FALSE.
v[1] is FALSE.
v[2] is TRUE.
v[3] is TRUE.
v[4] is NA.
TRUE 1
FALSE 0
NA_LOGICAL -2147483648
[1] FALSE FALSE  TRUE  TRUE    NA
```

## 論理演算

LogicalVector の要素ごとの論理演算には演算子 `&`（論理積） `|`（論理和） `!`（論理否定）を用いる。

```
LogicalVector v1 = {1,1,0,0};
LogicalVector v2 = {1,0,1,0};

LogicalVector res1 = v1 & v2;
LogicalVector res2 = v1 | v2;
LogicalVector res3 = !(v1 | v2);

Rcout << res1 << "\n"; // 1 0 0 0
Rcout << res2 << "\n"; // 1 1 1 0
Rcout << res3 << "\n"; // 0 0 0 1
```

## LogicalVector を受け取る関数

LogicalVector を受け取る関数には `all()` `any()` `ifelse()` がある。

### all() と any()

`LogicalVector` v に対して、`all(v)` は、v の全ての要素が `TRUE` の時 `TRUE` を返す。`any(v)` は、v のいずれかのの要素が `TRUE` の時、`TRUE`を返す。

`all()` と `any()` の返値の型は `bool` でも `int` でもなく `SingleLogicalResult` という特殊な型になっている。そのため `all()` や `any()` の返値を if 文の条件式として用いたり、bool 型に代入する場合には、`is_true()` `is_false()` `is_na()` を使って値を変換する必要がある。

```cpp
// [[Rcpp::export]]
List rcpp_logical_03(){
  LogicalVector v1 = LogicalVector::create(1,1,1,NA_LOGICAL);
  LogicalVector v2 = LogicalVector::create(0,1,0,NA_LOGICAL);
  
  // NA を含む LogicalVector に対する
  // all(), any() の挙動は R と同じ
  LogicalVector lv1 = all( v1 ); //NA
  LogicalVector lv2 = all( v2 ); //FALSE
  LogicalVector lv3 = any( v2 ); //TRUE 
  
  // bool に代入する場合
  bool b1 = is_true ( all(v1) );  // false
  bool b2 = is_false( all(v1) );  // false
  bool b3 = is_na   ( all(v1) );  // true

  // if 文の条件式で用いる場合
  //if(all( v2 )){       // 直接 if の条件式に入れるとエラー
  if(is_na(all( v1 ))) { // OK 
    Rcout << "all( v1 ) is NA\n";
  }
  
  return List::create(lv1, lv2, lv3, b1, b2, b3);
}
```

### ifelse()

`ifelse(v, x1, x2)` は論理ベクター v を受け取り、v の要素が `TRUE` の時には x1 の対応する要素を, `FALSE` の時には x2 の対応する要素を返す。x1, x2 はベクターでもスカラーでも良いが、ベクターの場合はその長さは v と一致している必要がある。

```cpp
// [[Rcpp::export]]
void rcpp_ifelse(){
  NumericVector   v1  = NumericVector::create(1,2,3,4,5);
  NumericVector   v2  = NumericVector::create(2,1,4,3,5);
  
  NumericVector res1 = ifelse(v1 > v2, 1, 0);
  NumericVector res2 = ifelse(v1 > v2, v1, 0);
  NumericVector res3 = ifelse(v1 > v2, v1, v2);
  
  Rcout << "res1 : " << res1 << "\n"; // 0 1 0 1 0
  Rcout << "res2 : " << res2 << "\n"; // 0 2 0 4 0
  Rcout << "res3 : " << res3 << "\n"; // 2 2 4 4 5
}
```


次のテーブルに R の 論理ベクトル (`logical`) の要素の値と C++ の `int` と `bool` の値の対応関係を示している。



```cpp
// [[Rcpp::export]]
List rcpp_logical_01(){

  // int 型で初期化
  LogicalVector v1 =
      LogicalVector::create(0,1,-1,2,NA_LOGICAL); 
  // bool 型で初期化
  LogicalVector v2 =
      LogicalVector::create(true,false);
  
  // Rcout で値を表示させると
  // LogicalVector の要素は int であることがわかる
  Rcout << "v1 : " << v1 << "\n";
  Rcout << "v2 : " << v2 << "\n";  
  
  return List::create(v1, v2);
}
```

実行結果

```
> rcpp_logical_01()
v1 : 0 1 -1 2 -2147483648
v2 : 1 0
[[1]]
[1] FALSE  TRUE  TRUE  TRUE    NA

[[2]]
[1]  TRUE FALSE
```


