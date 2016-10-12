# Taking advantage of Rcpp

R is weak in some kinds of operations. If you need operations listed below, it is time to consider using Rcpp.

* Loop operation in which later iteration depends on previous iteration.
* Recurrent call of function by loop operation.
* Accessing each elements of vector/matrix.
* Changing size of vector dynamically.
* Operation that need advanced data structure and algorithm.
