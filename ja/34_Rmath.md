# R Mathematics Library

 R Mathematics Library はRが提供する数学や統計などの関数を提供するライブラリであり、Rmath.h ヘッダーに記述されています。Rmath 自体は独立したライブラリーとして機能するので、他のプログラムからでも利用できます。

Rcpp からも Rmath.h で定義された関数を呼び出す事ができます。それらは `R::` 名前空間の中で定義されています。

Rmath.h の関数はC言語で書かれており、ベクトル化されていない。Rcppはベクトル化された関数を提供しているので、通常はそちらを使えば良い。



R::
```c
double norm_rand(void) 	
double unif_rand(void)	
double exp_rand(void)

void pnorm_both(double x, double *cum, double *ccum, int lt, int lg)
void rmultinom(int n, double* prob, int k, int* rn)	

double dsignrank(double x, double n, int lg)			
double psignrank(double x, double n, int lt, int lg)		
double qsignrank(double x, double n, int lt, int lg)		
double rsignrank(double n)

double dwilcox(double x, double m, double n, int lg)		
double pwilcox(double q, double m, double n, int lt, int lg)	
double qwilcox(double x, double m, double n, int lt, int lg)	
double rwilcox(double m, double n)

double ptukey(double q, double rr, double cc, double df, int lt, int lg)	
double qtukey(double p, double rr, double cc, double df, int lt, int lg)


double log1pmx(double x)                  
double log1pexp(double x)                 
double lgamma1p(double a)                 

double logspace_add(double lx, double ly) 
double logspace_sub(double lx, double ly) 


double gammafn(double x)			
double lgammafn(double x)			
double lgammafn_sign(double x, int *sgn)	

void   dpsifn(double x, int n, int kode, int m, double *ans, int *nz, int *ierr)	
double psigamma(double x, double deriv)	
double digamma(double x)	
double trigamma(double x)	
double tetragamma(double x)	
double pentagamma(double x)	

double beta(double a, double b)	
double lbeta(double a, double b)	

double choose(double n, double k)	
double lchoose(double n, double k)	

double bessel_i(double x, double al, double ex)	
double bessel_j(double x, double al)			
double bessel_k(double x, double al, double ex)	
double bessel_y(double x, double al)			
double bessel_i_ex(double x, double al, double ex, double *bi)	
double bessel_j_ex(double x, double al, double *bj)			
double bessel_k_ex(double x, double al, double ex, double *bk)	
double bessel_y_ex(double x, double al, double *by)	

double hypot(double a, double b)	
double pythag(double a, double b)	

double expm1(double x); /* = exp(x)-1 {care for small x} */	
double log1p(double x); /* = log(1+x) {care for small x} */ 

int imax2(int x, int y)		
int imin2(int x, int y)	

double fmax2(double x, double y)	
double fmin2(double x, double y)	

double sign(double x)		

double fprec(double x, double dg)	
double fround(double x, double dg)	
double fsign(double x, double y)	
double ftrunc(double x)		
```

下は [d/p/q/r 関数](dpqr_functions.md) のページにも記述した

```cpp
double dnorm(double x, double mu, double sigma, int lg)              
double pnorm(double x, double mu, double sigma, int lt, int lg)      
double qnorm(double p, double mu, double sigma, int lt, int lg)      
double rnorm(double mu, double sigma) 

double dunif(double x, double a, double b, int lg)		
double punif(double x, double a, double b, int lt, int lg)   
double qunif(double p, double a, double b, int lt, int lg)   
double runif(double a, double b)                             

double dgamma(double x, double shp, double scl, int lg)	   
double pgamma(double x, double alp, double scl, int lt, int lg) 
double qgamma(double p, double alp, double scl, int lt, int lg) 
double rgamma(double a, double scl)

double dbeta(double x, double a, double b, int lg)         
double pbeta(double x, double p, double q, int lt, int lg) 
double qbeta(double a, double p, double q, int lt, int lg) 
double rbeta(double a, double b)                           

double dlnorm(double x, double ml, double sl, int lg)	 
double plnorm(double x, double ml, double sl, int lt, int lg) 
double qlnorm(double p, double ml, double sl, int lt, int lg) 
double rlnorm(double ml, double sl)

double dchisq(double x, double df, int lg)          
double pchisq(double x, double df, int lt, int lg)  
double qchisq(double p, double df, int lt, int lg)  
double rchisq(double df)                            

double dnchisq(double x, double df, double ncp, int lg)          
double pnchisq(double x, double df, double ncp, int lt, int lg)  
double qnchisq(double p, double df, double ncp, int lt, int lg)  
double rnchisq(double df, double lb)                             

double df(double x, double df1, double df2, int lg)		
double pf(double x, double df1, double df2, int lt, int lg)	
double qf(double p, double df1, double df2, int lt, int lg)	
double rf(double df1, double df2)				

double dt(double x, double n, int lg)			
double pt(double x, double n, int lt, int lg)		
double qt(double p, double n, int lt, int lg)		
double rt(double n)						

double dbinom(double x, double n, double p, int lg)	  	
double pbinom(double x, double n, double p, int lt, int lg)  
double qbinom(double p, double n, double m, int lt, int lg)  
double rbinom(double n, double p)

double dcauchy(double x, double lc, double sl, int lg)		
double pcauchy(double x, double lc, double sl, int lt, int lg)	
double qcauchy(double p, double lc, double sl, int lt, int lg)	
double rcauchy(double lc, double sl)					

double dexp(double x, double sl, int lg)		
double pexp(double x, double sl, int lt, int lg)	
double qexp(double p, double sl, int lt, int lg)	
double rexp(double sl)				

double dgeom(double x, double p, int lg)		
double pgeom(double x, double p, int lt, int lg)	
double qgeom(double p, double pb, int lt, int lg)	
double rgeom(double p)				

double dhyper(double x, double r, double b, double n, int lg)		
double phyper(double x, double r, double b, double n, int lt, int lg)	
double qhyper(double p, double r, double b, double n, int lt, int lg)	
double rhyper(double r, double b, double n)					

double dnbinom(double x, double sz, double pb, int lg)		
double pnbinom(double x, double sz, double pb, int lt, int lg)	
double qnbinom(double p, double sz, double pb, int lt, int lg)	
double rnbinom(double sz, double pb)					

double dnbinom_mu(double x, double sz, double mu, int lg)		
double pnbinom_mu(double x, double sz, double mu, int lt, int lg)	
double qnbinom_mu(double x, double sz, double mu, int lt, int lg)	
double rnbinom_mu(double sz, double mu)				

double dpois(double x, double lb, int lg)		
double ppois(double x, double lb, int lt, int lg)	
double qpois(double p, double lb, int lt, int lg)	
double rpois(double mu)				

double dlogis(double x, double lc, double sl, int lg)		
double plogis(double x, double lc, double sl, int lt, int lg)	
double qlogis(double p, double lc, double sl, int lt, int lg)	
double rlogis(double lc, double sl)					

double dnbeta(double x, double a, double b, double ncp, int lg)		
double pnbeta(double x, double a, double b, double ncp, int lt, int lg)	
double qnbeta(double p, double a, double b, double ncp, int lt, int lg)	
double rnbeta(double a, double b, double np)					

double dnf(double x, double df1, double df2, double ncp, int lg)		
double pnf(double x, double df1, double df2, double ncp, int lt, int lg)	
double qnf(double p, double df1, double df2, double ncp, int lt, int lg)	

double dnt(double x, double df, double ncp, int lg)		
double pnt(double x, double df, double ncp, int lt, int lg)	
double qnt(double p, double df, double ncp, int lt, int lg)	

double dweibull(double x, double sh, double sl, int lg)		
double pweibull(double x, double sh, double sl, int lt, int lg)	
double qweibull(double p, double sh, double sl, int lt, int lg)	
double rweibull(double sh, double sl)
```
