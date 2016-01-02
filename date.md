# Date

Jan 1, 1970

##作成

```
Date d;       //"1970-01-01"
Date d(1);    //"1970-01-01" + 1day //negative value is also allowed
Date d(1.1);  //"1970-01-01" + ceil(1.1)day 
Date( "2000/01/01", "%Y/%m/%d"); //default format is "%Y-%m-%d"
Date( 1, 2, 2000); // 2000-01-02 Date(mon, day, year)
Date( 2000, 1, 2); // 2000-01-02 Date(year, mon, day)

```

##メソッド

d.getDate()
d.getDay()
d.getMonth()
d.getYear()
d.getWeekday()
d.getYearday()
d.baseYear() //1900

m.is.na()

## 演算子
+,-,<,>,==,<=,>=,!=
