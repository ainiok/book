### Http压测工具 wrk
> github [https://github.com/wg/wrk](https://github.com/wg/wrk)

wrk支持大多数类UNIX系统，不支持windows。需要操作系统支持LuaJIT和OpenSS


### install 

git 下载

```
git clone https://github.com/wg/wrk

make 
```

源码

```
wget https://github.com/wg/wrk/archive/4.1.0.zip

unzip 4.1.0.zip && cd wrk-4.1.0 && make
```

默认情况下wrk会使用自带的LuaJIT和OpenSSL，如果你想使用系统已安装的版本，可以使用WITH_LUAJIT和WITH_OPENSSL这两个选项来指定它们的路径。比如：

`make WITH_LUAJIT=/usr/local/luajit WITH_OPENSSL=/usr/local/openssl`

### 基本使用

```
Usage: wrk <options> <url>                            
  Options:                                            
    -c, --connections <N>  Connections to keep open   
    -d, --duration    <T>  Duration of test           
    -t, --threads     <N>  Number of threads to use   
                                                      
    -s, --script      <S>  Load Lua script file       
    -H, --header      <H>  Add header to request      
        --latency          Print latency statistics   
        --timeout     <T>  Socket/request timeout     
    -v, --version          Print version details      
                                                      
  Numeric arguments may include a SI unit (1k, 1M, 1G)
  Time arguments may include a time unit (2s, 2m, 2h)
```

**中文**

```
使用方法: wrk <选项> <被测HTTP服务的URL>                            
  Options:                                            
    -c, --connections <N>  跟服务器建立并保持的TCP连接数量  
    -d, --duration    <T>  压测时间           
    -t, --threads     <N>  使用多少个线程进行压测   
                                                      
    -s, --script      <S>  指定Lua脚本路径       
    -H, --header      <H>  为每一个HTTP请求添加HTTP头      
        --latency          在压测结束后，打印延迟统计信息   
        --timeout     <T>  超时时间     
    -v, --version          打印正在使用的wrk的详细版本信息
                                                      
  <N>代表数字参数，支持国际单位 (1k, 1M, 1G)
  <T>代表时间参数，支持时间单位 (2s, 2m, 2h)
```

**例子**

```
./wrk -t8 -c200 -d30s --latency  "http://www.baidu.com

Running 30s test @ http://www.baidu.com
  8 threads and 200 connections (8线程200连接)
  Thread Stats   Avg    Stdev(标准差) Max   +/- Stdev
    Latency(延迟)   268.05ms  197.73ms   1.99s    85.26%
    Req/Sec   101.76     26.72   222.00     67.74%
  Latency Distribution
     50%  205.82ms
     75%  309.77ms
     90%  497.74ms
     99%    1.05s 
  24359 requests in 30.06s, 357.59MB read
  在30.06s内完成了24359个请求
  Socket errors: connect 0, read 39, write 0, timeout 6
Requests/sec:    810.48
Transfer/sec:     11.90MB
```