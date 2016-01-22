# Datetime

`Datetime` は `DatetimeVector` `DatetimeMatrix` の要素となるスカラー型である。

##作成

```
Datetime dt;         //"1970-01-01 00:00:00 UTC"
Datetime dt(10.1);   //"1970-01-01 00:00:00 UTC" + 10.1sec
Datetime dt("2000-01-01 00:00:00", "%Y-%m-%d %H:%M:%OS")
```

Datetime は、日時を協定世界時(UTC) `1970-01-01 00:00:00` からの秒数（実数）で管理している。

`Datetime dt(10.1)` は世界協定時 `1970-01-01 00:00:00 UTC` の `10.1`秒後の時点を表す。この値をR返すと実行者のタイムゾーンに変換された時刻として表示される。

`Datetime dt( str, format)` では、 形式 `fmt` を指定して、文字列 `str` を `Datetime` に変換する。この場合、`str` はユーザーのローカルなタイムゾーンの時刻として解釈される。

(formatの記号は Rヘルプの `?strptime` の記載内容と一致している？）


##演算子



##メソッド

**注意！**：これらのメソッドを使って出力される値は世界協定時の値であり、ユーザーのタイムゾーンの値とは異なる。使用する際には注意すること。（このページの末尾に記載したのコードの実行結果を参照すること）


####getFractionalTimestamp()
基準日からの秒数（実数値）。

####getMicroSeconds()

世界協定時のマイクロ秒

秒の小数点以下の値を 1/1000 秒単位で表記した値。 (0.1 sec = 100000 micro sec)

####getSeconds()
世界協定時の秒
####getMinutes()
世界協定時の分
####getHours()
世界協定時の時
####getDay()
世界協定時の日
####getMonth()
世界協定時の月
####getYear()
世界協定時の年
####getWeekday()
世界協定時の曜日
1=Sun 2=Mon 3=Tue 4=Wed 5=Thu 6=Sat
####getYearday()
1月1日を1として、年初からの日数を整数で表した値。
####is_na()

##コード例

以下のコードを日本標準時（JST）で実行した結果を示す。

```
// [[Rcpp::export]]
Datetime rcpp_datetime(){
  Datetime dt("2000-01-01 00:00:00", "%Y-%m-%d %H:%M:%S");
  
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

JST は UTC よりも +9時間進んでいる。

```
> rcpp_datetime()
getYear 1999
getMonth 12
getDay 31
getHours 15
getMinutes 0
getSeconds 0
getMicroSeconds 0
getWeekday 6
getYearday 365
getFractionalTimestamp 9.46652e+08
[1] "2000-01-01 JST"
```


