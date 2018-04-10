## android数据库

#### 1.数据库介绍

#### sqlite
大家都知道的数据库

#### WCDB
微信开源的基于sqlcipher的轻量数据库，跨平台，支持ios, android, mac平台，做了一层封装，sqlite框架，更好的编程体验。性能和sqlite相当。自带数据库加密和数据库修复功能，更好的支持中文全文搜索。


#### Realm
由Y Combinator孵化的创业团队开源出来的一款可以用于iOS(同样适用于Swift&Objective-C)和Android的跨平台移动数据库，简单易用，学习成本低。



### 2.性能对比
Realm必现使用事务，性能才能好一些，但是对单条插入来说，性能就差了。相比sqlite， 插入速度上比sqlite差，但是查询速度要远远好于sqlite。

WCDB比较均衡，插入速度比sqlite要快一些，查询速度和realm相当。所以性能上还是很让人惊喜。



### 3.参考资料

[[1] Realm、WCDB与SQLite移动数据库性能对比测试](https://blog.csdn.net/cloudox_/article/details/75012746)

[[2] WCDB-源码分析](http://xiangwangfeng.com/2018/01/08/WCDB-%E6%BA%90%E7%A0%81%E8%A7%A3%E6%9E%90/)