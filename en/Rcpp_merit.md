# Taking advantage of Rcpp

R is not so fast for some kinds of operations. If you need operations listed below, it is time to consider adopting Rcpp for your project.

* Loop operations that previous iterations depend on previous ones.
* Recuurent call of functions.
* Accesing individual elements of vectors/matrices.
* Dynamically changing size of vectors.
* Operations that need advanced data structute and algorithms.

