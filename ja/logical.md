# LogicalVector と 論理演算

## LogicalVector

 R の論理ベクトルの実体は、実は、整数ベクトルである。C++ の論理値型は `bool` であるので、`LogicalVector` の要素の型は `bool` であると思うかもしれないが、実際には `int` 型である。なぜこのようになっているかというと `bool` で表現できるのは `true` と `false` だけであるが、R の論理ベクトルの要素の値には `TRUE`, `FALSE` だけではなく `NA` もあり得るためである。

次のテーブルに R の 論理ベクトル (`logical`) の要素の値と C++ の `int` と `bool` の値の対応関係を示している。

|logical|int|bool|
|:---:|:---:|:---:|
|TRUE|0 以外の値|true|
|FALSE|0|false|
|NA|intの最小値|-|

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

##論理演算

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

`all(v)`

論理ベクター v を受け取り、全ての要素が TRUE の時 TRUE を返す。

`any(v)`

論理ベクター v を受け取り、いずれかのの要素が TRUE の時、TRUEを返す。

返値がNAになることにも対応するため、`all()` や `any()` の返値の型は `bool` や `int` ではなく `SingleLogicalResult` という特殊な型になっている。そのため `all()` や `any()` の返り値を if 文の条件式として用いたり、bool 型に代入する場合には、`is_true()` `is_false()` `is_na()` を使って変換する。

```
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

###　ifelse()

ifelse(v, x1, x2)

論理ベクター cond を受け取り、TRUEの時には x1, FALSE の時には x2 を返す。x1, x2 はベクターでもスカラーでも良いが、ベクターの場合が長さは v と一致している必要がある。

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