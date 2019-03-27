> 根据某天获取当天的时间戳

```
$date = "2018-10-10";
$start_time = strtotime($date);
$end_time = strtotime("$date +1 day -1 second");
```
