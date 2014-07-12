---
title: data.table包问题整理
author: c cm
layout: post
permalink: /data_table_package_QA/
categories: R语言
tags:
---
data.table包比tapply快10+倍，比==快100+倍, 比DF[i, j] <- value快500+倍，绝对是值得花精力去学习的一个包。

![值得花多少时间](http://imgs.xkcd.com/comics/is_it_worth_the_time.png)


对于熟悉SQL的同学，data.table使用方法可以简单地概括为：

`DT[where,select|update,group by][having][order by][ ]...[ ]`

## Q: 读入数据
```r
DT <- fread("filelocation")
DT2 <- data.table(read.table("filelocation"))
```

## Q: 新增/删除/更新变量
```r
# add
DT[, varnew := var1 + var2]
DT[, `:=`(varnew1 = var1 * var2, varnew2 = var1 / var2)]
DT[, c("varnew1", "varnew2") := list(tmp <- var1 * var2, tmp/100)]
# delete
DT[, var3 := NULL]
DT[, c("var4", "var5") := NULL]
# update
DT[, varupdate := ifelse(var1 > var2, 0, varupdate)]
DT[var1 > var2, varupdate := 0]
```
## Q：j中函数使用变量名 
```r
fun <-function(x,y,where=parent.frame()){
	return(get(x,where) * get(y,where))
	}
DT[, lapply(c('var1','var2'), fun, y ='var3', where=.SD)]
```