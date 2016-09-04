---
layout: post
title: "python查询12306列车时刻"
date: 2016-09-04 19:32:07 +0800
comments: true
categories: 
---

### 查询12306列车时刻

命令执行格式 python query_tickets.py [-gdzkt] from_station to_station date  
[option]:   
	-g 高铁  
	-d 动车  
	-z 直达  
	-k 普快  
	-t 特快  
	
from_station 和 to_station目前只支持拼音,后续研究下支持中文查询

date: 日期格式，yyyy-MM-dd

关于车站名称的解析，通过开发者工具可以看到下边这个文件，车站名对应的Code都在这个文件里，可以写个脚步将其解析成set,方便后续使用,stations_code.py是解析后的结果  
"src="/otn/resources/js/framework/station_name.js?station_version=1.8964" "

代码地址：https://github.com/zoro301/query_train_tickets  
执行效果： 

![查询结果](https://github.com/zoro301/query_train_tickets/blob/master/result.jpg)
