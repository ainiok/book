## Redis client 命令



`getset key value `  将指定key设置为value,并且返回key的旧值

`setex key seconds value ` 将value关联到key，并将key的过期时间设置为seconds

`setnx key value ` 只有key不存在时设置key的值

`setrange key offset value ` 用 value 参数覆写给定 key 所储存的字符串值，从偏移量 offset 开始

`incr key ` 将key中存储的数字值增1

`incrby key increment` 将key中存储的数字值加上给定增量值 increment

`incrbyfloat key increment` 将key中存储的数字值加上给定浮点增量值 increment

`decr key ` 将key中存储的数字减1

`decrby key decrement `

`decrbyfloat key decrement `

## Hash

`hmset key field value [field value]` 

`hdel key field1 [field2]`  删除一个或多个哈希表字段

`hget key filed`

`hgetall key `

`hexists key field `

## List

`lpush key value [value] `  将一个值或多个值插入到列表头部

`rpush key value [value] `

`lpop key`

`rpop key`