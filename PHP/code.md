> 根据某天获取当天的时间戳

```
$date = "2018-10-10";
$start_time = strtotime($date);
$end_time = strtotime("$date +1 day -1 second");
```


> 求n以内的质数

```php
function get_prime($n)
{
    if ($n<2) return [];
    $prime = array(2);//2为质数

    for ($i = 3; $i <= $n; $i += 2) {//偶数不是质数，步长可以加大 
        $sqrt = intval(sqrt($i));//求根号n

        for ($j = 3; $j <= $sqrt; $j += 2) {//i是奇数，当然不能被偶数整除，步长也可以加大。 
            if ($i % $j == 0) {
                break;
            }
        }

        if ($j > $sqrt) {
            array_push($prime, $i);
        }
    }

    return $prime;
}
```