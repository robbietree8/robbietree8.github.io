---
title: influxdb and grafana
date: 2019-03-15
draft: false
---

# InfluxDB & Grafana 趟坑过程记录


## InfluxDB

> 子查询查最新版本号的分布情况

```sql
select count(*) from (select count(value) from table where app =~ /^abc/ group by "app", "version" order by time desc limit 1) group by "app", "version" order by time desc
```

在子查询里，已经按照时间拿最新的一条数据，但是在外面再套一层之后，老的数据又出来了，这个问题暂时没解，而是通过新的数据过来之后，删掉老的数据规避掉。


> 时区问题

默认情况下，`InfluxDB`使用UTC来计算时间区块的数据，可以使用`tz()`子句来指定时区，比如如下指定了`UTC+8`时区

```sql
SELECT sum("value") FROM "table" GROUP BY time($__interval) fill(null) tz('Asia/Shanghai')
```



## Grafana

> Variables - Include All option

Grafana里可以配置变量，后续变量可以在绘制Dashboard的时候使用，如下图所示

__配置界面__

![](https://i.loli.net/2019/03/15/5c8bac8c90593.jpg)

__使用界面__

![](https://i.loli.net/2019/03/15/5c8bac8fbc0f4.jpg)


注意其中的`Include All option`，选中这个勾选框之后，下拉界面就能出现`All`这个选项，作用是查询语句将会使用(A|B|C)作为查询条件

在下拉选项少的情况下，这样用是一点问题也没有的，但是如果下拉选项很多的话，就会有问题，`Grafana`查询数据的时候是用`GET`方法查询的，即查询语句会出现在URL里，带来的一个问题是，如果走`nginx`的话，会被`nginx`拒绝掉，原因是nginx对URL的长度有限制。当然可以调整`nginx`配置来规避这个问题，还有一种办法是，通过自定义All的语义来更**优雅**的解决，如上所示，这边个性化了`Custom all value`为`.*`，熟悉正则的朋友肯定已经懂了，po个查询语句

```sql
SELECT sum("value") FROM "table" WHERE ("level" =~ /^$level$/ AND $timeFilter GROUP BY time($__interval) fill(null)
```

> 级连查询

`Grafana`支持你做级连查询，想一下省市级连就知道了

定义的方法如下

首先定义一个变量，命名为`level`

```
SHOW TAG VALUES FROM "table" WITH KEY = "level"
```

接着再定义一个变量，命名为`title`

```
SHOW TAG VALUES FROM "table" WITH KEY = "level" where "level" =~ /^level$/
```

> Legend Avg

假如查询出来的series包含null值并且在Null value的配置里配置了`null as zero`的话，平均值计算出来的就会不对，它会将null值也累加到分母里，所以这里需要将Null value配置成`null`，来让前端忽略这个null值，参见`Ignore nulls unless 'null as zero' for series.stats.avg`

> Group By Tags

在列比较稀疏的情况下，比如，第一次写入的数据是A、B、C三列，第二次写入的数据是A、D、E三列，tag为X列，按照X列做group by，如果选择的字段是B列，则第一次数据的tag不会出现，只有选择两个公有的列，这里是A列，两次写入的数据的tag才会出现。

> 如何隐藏Legend

可以在Display里配置是否展示 Legend，具体配置见下图所示

![](https://i.loli.net/2019/05/30/5cef451c1f0e642733.jpg)

> 环比、同比

InfluxDB 提供了`derivative()`函数用来计算变化速率
e.x.

__raw_data__
```
name: h2o_feet
time                   water_level
----                   -----------
2015-08-18T00:00:00Z   2.064
2015-08-18T00:06:00Z   2.116
2015-08-18T00:12:00Z   2.028
2015-08-18T00:18:00Z   2.126
2015-08-18T00:24:00Z   2.041
2015-08-18T00:30:00Z   2.051
```

```sql
SELECT DERIVATIVE("water_level",6m) FROM "h2o_feet" WHERE "location" = 'santa_monica' AND time >= '2015-08-18T00:00:00Z' AND time <= '2015-08-18T00:30:00Z'
```

```
name: h2o_feet
time			derivative
----			----------
2015-08-18T00:06:00Z	0.052000000000000046
2015-08-18T00:12:00Z	-0.08800000000000008
2015-08-18T00:18:00Z	0.09799999999999986
2015-08-18T00:24:00Z	-0.08499999999999996
2015-08-18T00:30:00Z	0.010000000000000231
```

计算6分钟的变化速率
```
(2.116 - 2.064) / (6m / 6m)
--------------    ----------
       |              |
       |          the difference between the field values' timestamps / the specified unit
second field value - first field value
```

## 参考资料

- [Request-URI Too Large](https://github.com/grafana/grafana/issues/8109)
- [Add support for InfluxDB `tz` clause](https://github.com/grafana/grafana/issues/10322)
- [The Time Zone Clause](https://docs.influxdata.com/influxdb/v1.4/query_language/data_exploration/#the-time-zone-clause)
- [Ignore nulls unless 'null as zero' for series.stats.avg](https://github.com/grafana/grafana/pull/3252)
