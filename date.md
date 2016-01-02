# Date

Jan 1, 1970

作成

```
Date d;       //"1970-01-01"
Date d(1);    //"1970-01-01" + 1day //negative value is also allowed
Date d(1.1);  //"1970-01-01" + ceil(1.1)day 
Date d(string, format);

Date( "2000/01/01", "%Y/%m/%d"); //default format is "%Y-%m-%d"

```
