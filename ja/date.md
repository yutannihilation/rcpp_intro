# Date

`Date` は、`DateVector` の要素となる、スカラー型です。


##作成

```cpp
Date d;       //"1970-01-01"
Date d(1);    //"1970-01-01" + 1day
Date d(1.1);  //"1970-01-01" + ceil(1.1)day 
Date( "2000-01-01", "%Y-%m-%d"); //default format is "%Y-%m-%d"
Date( 1, 2, 2000); // 2000-01-02 Date(mon, day, year)
Date( 2000, 1, 2); // 2000-01-02 Date(year, mon, day)
```
##演算子

`Date` には 以下の演算子が定義されています。

`+ - < > >= <= == !=`

```
// [[Rcpp::export]]
DateVector rcpp_date1(){
  
  Date d1("2000-01-01");
  Date d2("2000-02-01");
  
  DateVector date(1);
  
  int  i  = d2 - d1;
  bool b  = d2 > d1;
  date[0] = d1 + 1;
  
  Rcout << i << endl; // 31
  Rcout << b << endl; // 1
  return date;        // 2000-01-02
}
````



##メソッド


####getDay()



####getMonth()



####getYear()



####getWeekday()

曜日を int で返す。
1=Sun 2=Mon 3=Tue 4=Wed 5=Thu 6=Sat

####getYearday()

1月1日を 1 とした年間を通した日付の番号

####is.na()

##実行例

```
Date d("2016-1-1");
Rcout << d.getDay() << endl;     //1
Rcout << d.getMonth() << endl;   //1
Rcout << d.getYear() << endl;    //2016
Rcout << d.getWeekday() << endl; //6
Rcout << d.getYearday() << endl; //1
```
















