# Datetime

##作成

`Datetime` は `DatetimeVector` `DatetimeMatrix` の要素となるスカラー型。


```
Datetime dt;         //"1970-01-01 00:00:00 UTC"
Datetime dt(10.1);   //"1970-01-01 00:00:00 UTC" + 10.1sec
Datetime dt("2000-01-01 00:00:00", "%Y-%m-%d %H:%M:%OS")
```

Datetime では、協定世界時 `1970-01-01 00:00:00 UTC` を基準時として、日時を基準時からの秒数で管理している。

`Datetime dt(10.1)` は世界協定時 `1970-01-01 00:00:00 UTC` の `10.1`秒後の時点を表す。Rでは実行者のタイムゾーンに変換された時刻として表示される。

`Datetime dt( str, fmt)` では、 形式 `fmt` を指定して、文字列 `str` を `Datetime` に変換する。この場合、`str` はユーザーのタイムゾーンの時刻として解釈される。format については Rヘルプの `?strptime` を参照する。





##メソッド

####getFractionalTimestamp()
基準日からの秒数（実数値）。


get

####getMicroSeconds()

小数点以下を1/1000秒単位で表記 (0.1 sec = 100000 micro sec)

####getSeconds()
####getMinutes()
####getHours()
####getDay()
####getMonth()
####getYear()
####getWeekday()
####getYearday()



```
// [[Rcpp::export]]
Datetime rcpp_datetime(){
  Datetime dt("2011-01-01 00:00:00", "%Y-%m-%d %H:%M:%S");
  
  Rcout << "getYear " << dt.getYear() << endl;
  Rcout << "getMonth " << dt.getMonth() << endl;
  Rcout << "getDay " << dt.getDay() << endl;
  
  Rcout << "getHours " << dt.getHours() << endl;
  Rcout << "getMinutes " << dt.getMinutes() << endl;
  Rcout << "getSeconds " << dt.getSeconds() << endl;
  
  Rcout << "getMicroSeconds " << dt.getMicroSeconds() << endl;
  Rcout << "getWeekday " << dt.getWeekday() << endl;
  Rcout << "getYearday " << dt.getYearday() << endl;
  Rcout << "getFractionalTimestamp " << dt.getFractionalTimestamp() << endl;
  
  return dt;
}
```

実行結果
```
> rcpp_datetime()
getYear 2010
getMonth 12
getDay 31
getHours 15
getMinutes 0
getSeconds 0
getMicroSeconds 0
getWeekday 6
getYearday 365
getFractionalTimestamp 1.29381e+09
[1] "2011-01-01 JST"
```


