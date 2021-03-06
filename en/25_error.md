# Error handling

In a situation in which the normal progress of the program is hindered, you can print an error message and stop the program. If you want to warn the user without stopping the program, use `warning()` function. Both functions `stop()` and `warning()` can display messages by specifying the format as same as the `Rprintf()` function.


```
stop("Error: Unexpected condition occurred");
stop("Error: Column %i is not numeric.", i+1);

warning("Warning: Unexpected condition occurred");
warning("Warning: Column %i is not numeric.", i+1);
```

In the code example below, an error is printed and execution is stopped if the value given to the function is negative.

```
// [[Rcpp::export]]
double rcpp_log(double x) {
    if (x <= 0.0) {
        stop("'x' must be a positive value.");
    }
    return log(x);
}
```

Execution result

```
> rcpp_log(-1)
 Error: 'x' must be a positive value.
```


# Throwing C++ exception

By using you can throw a C++ exception.

```
throw exception("Unexpected condition occurred");
```
