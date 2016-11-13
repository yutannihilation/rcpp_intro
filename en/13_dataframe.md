# DataFrame

## Creating a DataFrame object

`DataFrame::create()` is used to create a `DataFrame` object. Also, use `Named()` or `_[]` if you want to specify column names when creating `DataFrame`　object.

```cpp
// Creating DataFrame df from Vector v1, v2
DataFrame df = DataFrame::create(v1, v2);
// When giving names to columns
DataFrame df = DataFrame::create( Named("V1") = v1 , _["V2"] = v2 );
```

When you create a `DataFrame` with` DataFrame::create() `, the value of the original` Vector` element will not be duplicated in the columns of the `DataFrame`, but the columns will be the "reference" to the original `Vector`. Therefore, changing the value of the original `Vector` changes the value of the columns. To avoid this, we use the `clone()` function to duplicate the value of the `Vector` element when creating a `DataFrame` column.

To see the difference between using the `clone()` function and not using it, see the code example below. In the code example, we are creating `DataFrame` df from `Vector` v. There, column V1 is a reference to v, and column V2 replicates the value of v by the `clone ()` function. After that, if you change to `Vector` v, the values of column V1 is changed, but V2 is not affected.

``` cpp
// [[Rcpp::export]]
DataFrame rcpp_df(){
    // Creating vector v
    NumericVector v = {1,2};
    // Creating DataFrame df
    DataFrame df = DataFrame::create( Named("V1") = v,         // simple assign
                                      Named("V2") = clone(v)); // using clone()
    // Changing vector v
    v = v * 2;
    return df;
}
```

Execution result

```
> rcpp_df()
  V1 V2
1  2  1
2  4  2
```




## Accessing DataFrame elements

When accessing a specific column of `DataFrame`, the column is temporarily assigned to `Vector` object and accessed via the object. As with `Vector`, the `DataFrame` column can be specified by a numeric vector (column number), a string vector (column name), and a logical vector.

```
NumericVector v1 = df[0];
NumericVector v2 = df["V2"];
```

As with `DataFrame` creation, assigning a` DataFrame` column to `Vector` in the above way will not copy the column  value to `Vector` object, but it will be a "reference" to the column. Therefore, when you change the values of `Vector` object, the content of the column will also be changed.

If you want to create a `Vector` by copying the value of the column, use `clone()` function so that the value of the original `DataFrame` column is not changed.

```
NumericVector v1 = df[0]; // v1 は dfの 0 列目への「参照」となります
v1 = v1*2;                // v1 の値を変更すると df[0] の値も変更されます

NumericVector v2 = clone(df[0]); // v2 には df[0] の要素の値を複製します
v2 = v2*2;                       // v2 を変更しても df[0] の値は変わりません
```



##メンバ関数

Rcpp では、`DataFrame` や `List` は、ある種のベクトルとして実装されています。つまり、`Vector` は、スカラー値を要素とするベクトル、`DataFrame` は同じ長さの `Vector` を要素とするベクトルです。そのため、`DataFrame` は `Vector` 共通するメンバ関数を多く持っています。


#### length() size()

列数を返します。


####nrows()

行数を返します。

####names()

カラム名を文字列ベクトルで返します。

####offset(name) findName(name)

文字列 name で指定された名前のカラムの列番号を返します。

####findName(name)

文字列 name で指定された名前のカラムの列番号を返します。


####fill(v)

この `DataFrame` の全てのカラムを `Vector` v で満たします。


####assign( first_it, last_it)

イテレーター first_it, last_it で指定された範囲のカラムを、この `DataFrame` に代入します。

####push_back(v)

この `DataFrame`  の末尾に `Vector` v を追加します。

####push_back( v, name )

この `DataFrame`  の末尾に `Vector` v を追加します。 追加したカラムの名前を文字列 name で指定します。

####push_front(x)

この `DataFrame`  の先頭に `Vector` v を追加します。


####push_front( x, name )

この `DataFrame`  の先頭に `Vector` v を追加します。追加したカラムの名前を文字列 name で指定します。

#### begin()

この `DataFrame` の先頭カラムへのイテレータを返します。

#### end()

この `DataFrame` の末尾へのイテレータを返します。

#### insert( it, v )

この `DataFrame` の、イテレータ it で示された位置に``Vector` ` v を追加し、その要素へのイテレータを返します。

#### erase(i)

この `DataFrame` の i 番目のカラムを削除し、削除した直後のカラムへのイテレータを返す。

####erase(it)

イテレータ it で指定されたカラムを削除し、削除した直後のカラムへのイテレータを返します。

####erase(first_i, last_i)

first_i 番目 から last_i -1 番目 までのカラムを削除し、削除した直後のカラムへのイテレータを返します。


#### erase(first_it, last_it)

イテレータ first_it から last_it -1 で指定されるカラムまでを削除し、削除した直後のカラムへのイテレータを返します。

#### containsElementNamed(name)

この `DataFrame` が文字列 name で指定された名前のカラムを持っている場合には true を返します。

#### inherits(str)

このオブジェクトの属性 class に文字列 str が含まれているかどうか。
