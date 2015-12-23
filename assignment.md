#Rcppオブジェクトで代入する際の注意点

```
NumericVector
myFunc(NumericVector A){
   A[0]=100;
 NumericVector B=A;
 B[1]=200;
  return(A);
}//A==c(100,200,3,4,5) B への変更が A に影響する
```

これはB=Aとした時に、BにはAの値をコピーされるのではなく、BはAへの「参照」として代入されるため（AとBは同じオブジェクトを指している）。Bへのアクセスすると結局Aへアクセスすることになるため。
下のように、clone を使うと B へ A の値がコピーされる。AとBは別のオブジェクトとなる。

```
NumericVector
myFunc(NumericVector A){
   A[0]=100;
 NumericVector B=clone(A);
 B[1]=200;
  return(A);
}//A==c(100,200,3,4,5) B への変更が A に影響する
```


Rcpp のオブジェクト間で = を使うときには、「参照」として代入されているのか、「値をコピー」して代入されているのか注意する必要がある。

