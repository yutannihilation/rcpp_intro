# Date

`Date` はスカラー型で、`DateVector`, `DateMatrix` の要素となる。


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

`+ - < >`

`
Date d1("2000-01-01");
Date d1("2000-02-01");

Rcout << d1 + 1 << endl;

`



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
















