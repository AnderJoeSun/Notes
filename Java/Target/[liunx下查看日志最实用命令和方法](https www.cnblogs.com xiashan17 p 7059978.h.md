---
tags: [未看]
---

# [liunx下查看日志最实用命令和方法](https://www.cnblogs.com/xiashan17/p/7059978.html)

1.业务系统访问量不是很大的时候，使用这个，有bug的地方操作下，直接看最后操作的日志，就是你刚才操作的地方，好好查bug吧

tail  -fn100  catalina.log   查询日志尾部最后100行的日志,并且随文件;

2.查看服务器启动情况，服务器启动报错，直接看前n行即可

head -n100  catalina.log   查询日志文件中的头10行日志;

3.按照关键字查找日志 （知道程序出问题的模块，而且有日志关键字的可以用此方法）

<1>.找到发错错误异常的行号

比如我们日志中关键字error表示错误

grep "error" -n access.log

或者cat -n catalina.log |grep "error" 

这时候就会显示很多匹配的行数，然后找到大约发生错误时间的对应行号

<2>通过行号查询对应行前后的内容

例如：得到"error"关键字所在的行号是102行. 此时如果我想查看这个关键字前10行和后10行的日志:

cat -n catalina.log |tail -n +92|head -n 20

tail -n +92表示查询92行之后的日志

head -n 20 则表示在前面的查询结果里再查前20条记录

或者 sed -n "92,112p" catalina.log

sed -n "开始行,结束行p" 文件名 查看文件多少行到多少行内容

4.通过时间查找 （不知道程序那里出问题了，只知道出问题的时间）

查询一个时间字符串是否存在

grep “2017-06-21 10:00” test.log

查询时间段内的日志

sed -n '/2017-06-21 09:25:55/,/2017-06-21 14:25:55/p' access.log

这个方法网上都说这个搞，但实际上我实践的时候不能查出来什么，不知道为什么，如果不行只能查时间字符串

grep "2017-06-21 09:25:55" -n access.log

cat -n test.log |grep "error" |more

5.查询日志结果如果太多可以分页到导出文件

<1>使用more和less命令, 如: cat -n test.log |grep "error" |more     这样就分页打印了,通过点击空格键翻页

<2>使用 >look.txt 将其保存到文件中,到时可以拉下这个文件分析.如:

cat -n test.log |grep "地形"  >look.txt

6.日志管理工具，以上方式只能解决服务器单节点问题，多节点日志分析不建议在服务器上一个个节点去查看，通常简单的是运维定时合并同一业务类型日志到某一个目录

同时也有一些开源的日志管理软件可以帮你管理日志，很简单的帮你实现分析，搜索

如开源的Graylog 2 Logstash Sumo Logic 收费的 Splunk  等