---
title: "Apache Bench"
layout: page
date: 2016-06-07 00:00
---

[TOC]

### centos和ubuntu单独安装ab
```shell
apt-get install apache2-utils

yum install httpd-tools
```

### 参数说明

```shell
junpeng@localhost:/var/www$ ab
ab: wrong number of arguments
Usage: ab [options] [http[s]://]hostname[:port]/path
Options are:
    -n requests     Number of requests to perform
                    //在测试会话中所执行的请求个数（本次测试总共要访问页面的次数）。默认时，仅执行一个请求。
    -c concurrency  Number of multiple requests to make at a time
                    //一次产生的请求个数（并发数）。默认是一次一个。
    -t timelimit    Seconds to max. to spend on benchmarking
                    This implies -n 50000
                    //测试所进行的最大秒数。其内部隐含值是-n 50000。它可以使对服务器的测试限制在一个固定的总时间以内。默认时，没有时间限制。
    -s timeout      Seconds to max. wait for each response
                    Default is 30 seconds

    -b windowsize   Size of TCP send/receive buffer, in bytes
    -B address      Address to bind to when making outgoing connections
    -p postfile     File containing data to POST. Remember also to set -T
                    //包含了需要POST的数据的文件，文件格式如“p1=1&p2=2”.使用方法是 -p 111.txt 。 （配合-T）
    -u putfile      File containing data to PUT. Remember also to set -T
    -T content-type Content-type header to use for POST/PUT data, eg.
                    'application/x-www-form-urlencoded'
                    Default is 'text/plain'
                    //POST数据所使用的Content-type头信息，如 -T “application/x-www-form-urlencoded” 。 （配合-p）
    -v verbosity    How much troubleshooting info to print
                    //设置显示信息的详细程度 – 4或更大值会显示头信息， 3或更大值可以显示响应代码(404, 200等), 2或更大值可以显示警告和其他信息。 -V 显示版本号并退出。
    -w              Print out results in HTML tables
                    //以HTML表的格式输出结果。默认时，它是白色背景的两列宽度的一张表。
    -i              Use HEAD instead of GET
                    // 执行HEAD请求，而不是GET。
    -x attributes   String to insert as table attributes
    -y attributes   String to insert as tr attributes
    -z attributes   String to insert as td or th attributes
    -C attribute    Add cookie, eg. 'Apache=1234'. (repeatable)
                    //-C cookie-name=value 对请求附加一个Cookie:行。 其典型形式是name=value的一个参数对。此参数可以重复，用逗号分割。
                    //提示：可以借助session实现原理传递 JSESSIONID参数， 实现保持会话的功能，如:-C ” c1=1234,c2=2,c3=3, JSESSIONID=FF056CD16DA9D71CB131C1D56F0319F8″ 。
    -H attribute    Add Arbitrary header line, eg. 'Accept-Encoding: gzip'
                    Inserted after all normal header lines. (repeatable)
    -A attribute    Add Basic WWW Authentication, the attributes
                    are a colon separated username and password.
    -P attribute    Add Basic Proxy Authentication, the attributes
                    are a colon separated username and password.
                    //-P proxy-auth-username:password 对一个中转代理提供BASIC认证信任。用户名和密码由一个:隔开，并以base64编码形式发送。无论服务器是否需要(即, 是否发送了401认证需求代码)，此字符串都会被发送。
    -X proxy:port   Proxyserver and port number to use
    -V              Print version number and exit
    -k              Use HTTP KeepAlive feature
    -d              Do not show percentiles served table.
    -S              Do not show confidence estimators and warnings.
    -q              Do not show progress when doing more than 150 requests
    -l              Accept variable document length (use this for dynamic pages)
    -g filename     Output collected data to gnuplot format file.
    -e filename     Output CSV file with percentages served
    -r              Don't exit on socket receive errors.
    -m method       Method name
    -h              Display usage information (this message)
    -Z ciphersuite  Specify SSL/TLS cipher suite (See openssl ciphers)
    -f protocol     Specify SSL/TLS protocol
                    (SSL3, TLS1, TLS1.1, TLS1.2 or ALL)

    //-attributes 设置属性的字符串. 缺陷程序中有各种静态声明的固定长度的缓冲区。另外，对命令行参数、服务器的响应头和其他外部输入的解析也很简单，这可能会有不良后果。它没有完整地实现 HTTP/1.x; 仅接受某些’预想’的响应格式。 strstr(3)的频繁使用可能会带来性能问题，即你可能是在测试ab而不是服务器的性能。


```

### 用法

装好后执行:
```shell
ab -n 4000 -c 1000 http://www.baidu.com/
```
-n 后面的4000代表总共发出4000个请求;

-c 后面的1000表示采用1000个并发（模拟1000个人同时访问）;

最后为测试的url;

### 结果分析

```shell
junpeng@localhost:/var/www$ ab -n 100 -c 10 http://qlocenter.nxgrpc.net/Dashboard/abstracts
This is ApacheBench, Version 2.3 <$Revision: 1604373 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/
Benchmarking qlocenter.nxgrpc.net (be patient).....done
Server Software:        nginx/1.1.19
Server Hostname:        qlocenter.nxgrpc.net
Server Port:            80
Document Path:          /Dashboard/abstracts
    //测试的页面
Document Length:        12013 bytes
    //页面大小
Concurrency Level:      10
    //测试的并发数
Time taken for tests:   1.709 seconds
    //整个测试持续的时间
Complete requests:      100
    //完成的请求数量
Failed requests:        9
    //失败的请求数量
   (Connect: 0, Receive: 0, Length: 9, Exceptions: 0)
Non-2xx responses:      1
Total transferred:      1111832 bytes
    //整个过程中的网络传输量
HTML transferred:       1093352 bytes
    //整个过程中的HTML内容传输量
Requests per second:    58.52 [#/sec] (mean)
    //最重要的指标之一，相当于LR中的每秒事务数，后面括号中的mean表示这是一个平均值
Time per request:       170.881 [ms] (mean)
    //最重要的指标之二，相当于LR中的平均事务响应时间，后面括号中的mean表示这是一个平均值
Time per request:       17.088 [ms] (mean, across all concurrent requests)
    //每个连接请求实际运行时间的平均值
Transfer rate:          635.40 [Kbytes/sec] received
    //平均每秒网络上的流量，可以帮助排除是否存在网络流量过大导致响应时间延长的问题
Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        1    1   0.6      1       4
Processing:   109  164  77.9    132     438
Waiting:      104  158  77.2    127     436
Total:        110  166  78.1    133     440
    //网络上消耗的时间的分解
      横轴栏位的部分:
        min:      最小值
        mean:     平均值(正/负标准差)
        median:   平均值(中间值)
        max:      最大值

      纵轴栏位的部分:
        Connect:  从ab发出TCP要求到Web主机所花费的建立时间.
        Processing: 从TCP连线建立后,直到HTTP回应(Response)的数据全部收到所花的时间.
        Waiting: 从发送HTTP要求完后,到HTTP回应(Response)第一个Byte所等待的时间.
        Total: 等于Connect + Processing 的时间(因为Waiting包含在Processing时间内了)

Percentage of the requests served within a certain time (ms)
  50%    133
  66%    141
  75%    162
  80%    183
  90%    286
  95%    403
  98%    439
  99%    440
 100%    440 (longest request)
     //整个场景中所有请求的响应情况。
     //在场景中每个请求都有一个响应时间，其中50％的用户响应时间小于133毫秒，66％的用户响应时间小于141毫秒，最大的响应时间小于440毫秒。
     //对于并发请求，cpu实际上并不是同时处理的，而是按照每个请求获得的时间片逐个轮转处理的，所以基本上第一个Time per request时间约等于第二个Time per request时间乘以并发请求数。
```
