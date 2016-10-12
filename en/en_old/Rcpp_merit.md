# Taking advantage of Rcpp

R is weak in some kinds of operations. If you need operations listed below, it is time to consider using Rcpp.

* Loop operations that following iteration depends on the previous iteration.
* Recurrent call of functions.
* Accessing individual elements of vectors/matrices.
* Dynamically changing size of vectors.
* Operations that need advanced data structures and algorithms.
