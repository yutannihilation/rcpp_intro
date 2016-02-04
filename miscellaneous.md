# その他

### ユーザーからの処理のキャンセルを受け付ける。

`checkUserInterrupt()` は処理の実行途中でユーザーがキャンセル（ctrl + c）ボタンが押されたかどうかを確認し、押されていた場合には処理を中断する。長時間を要する処理を実行する場合には、おおよそ数秒に１回程度の頻度で `checkUserInterrupt()` が実行されるようにすると良いだろう。

```
for (int i=0; i<1000000; ++i) {
  // check for interrupt every 1000 iterations
  if (i % 1000 == 0)
    Rcpp::checkUserInterrupt();
  // ...do some expensive work...
}

```