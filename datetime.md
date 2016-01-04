# Datetime

##作成

```
Datetime dt; //"1970-01-01 00:00:00 UTC"
Datetime dt(10.1); //"1970-01-01 09:00:00 JST" + 10.1sec
Datetime dt("2000-01-01 00:00:00", "%Y-%m-%d %H:%M:%OS")
```


##メソッド

dt.getFractionalTimestamp()

dt.getMicroSeconds()
dt.getSeconds()
dt.getMinutes()
dt.getHours()
dt.getDay()
dt.getMonth()
dt.getYear()
dt.getWeekday()
dt.getYearday()

dt.is_na()