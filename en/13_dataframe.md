# DataFrame

This chapter explains how to create `DataFrame` object, how to access its elements, and its member functions. In Rcpp, `DataFrame` is implemented as a kind of vector. In other words, `Vector` is a vector whose element is scalar value, while `DataFrame` is a vector whose elements are `Vector`s of the same length. Therefore, `Vector` and `DataFrame` have many common methods of creating objects, accessing elements, and member functions.

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
NumericVector v1 = df[0]; // v1 becomes "reference" to the 0th column of df
v1 = v1 * 2;              // Changing the value of v1 also changes the value of df[0]

NumericVector v2 = clone(df[0]); // Duplicate the value of the element of df[0] to v2
v2 = v2*2;                       // Changing v2 does not change the value of df [0]
```



## Member functions

In Rcpp, `DataFrame` is implemented as certain kinds of vectors. In other words, `Vector` is a vector whose elements are scalar values, and `DataFrame` is a vector whose elements are `Vector`s. Therefore, `DataFrame` has many member functions common to `Vector`.

#### length() size()

Returns the number of columns.


#### nrows()

Returns the number of rows.

#### names()

Returns the column name as a CharacterVector.

#### offset(name) findName(name)

Returns the numerical index of the column with the name specified by the string "name".

#### fill(v)

fills all the columns of this `DataFrame` with` Vector` v.

#### assign( first_it, last_it)

Assign columns in the range specified by the iterators first_it and last_it to this `DataFrame`.

#### push_back(v)

Add `Vector` v to the end of this` DataFrame`.

#### push_back( v, name )

Append a `Vector` v to the end of this` DataFrame`. Specify the name of the added column with the string "name".

#### push_front(x)

Append a `Vector` v at the beginning of this` DataFrame`.


#### push_front( x, name )

Append a `Vector` v at the beginning of this` DataFrame`. Specify the name of the added column with the string "name".

#### begin()

Returns an iterator pointing to the first column of this `DataFrame`.

#### end()

Return an iterator pointing to the end of this `DataFrame`.

#### insert( it, v )

Add `Vector` v to this `DataFrame` at the position pointed by the iterator `it` and return an iterator to that element.

#### erase(i)

Delete the `i`th column of this `DataFrame` and return an iterator to the column just after erased column.

#### erase(it)

Deletes the column specified by the iterator `it` and returns an iterator to the column just after erased column.

####erase(first_i, last_i)

Deletes the `first_i`th to `last_i - 1`th columns and returns an iterator to the column just after erased column.

#### erase(first_it, last_it)

Deletes the range of columns from those specified by the iterator `first_it` to those specified by `last_it - 1` and returns an iterator to the column just after the erased columns.

#### containsElementNamed(name)

Returns true if this `DataFrame` has a column with the name specified by the string `name`.

#### inherits(str)

Returns true if the attribute "class" of this object contains the string `str`.
