
```sql
# 条件查询SQL结构
SELECT [fields] FROM [index] WHERE [bool query condition] ORDER BY [sort field] LIMIT [0, 10]

# 聚合查询SQL结构
SELECT [MIN(field),MAX(field),SUM(field),AVG(field)] from [index.type] WHERE [bool query condition] GROUP BY [agg methods]
```
以上两种SQL结构分别对应ES的条件查询请求和聚合查询请求，下面大概看看这两种结构，后续会详细介绍。

1、条件查询请求SQL

* SELECT项，可以是通配符* 也可以是具体指定需要返回的字段（针对inner文档直接通过.引用）
* FROM项，后面跟的是索引名
* WHERE项，进行复杂的bool匹配
* ORDER BY项，跟SQL一样后面跟排序条件
* LIMIT 项，指定分页参数对应ES的from，size参数

2、 聚合查询请求SQL

* SELECT/FROM/WHERE 项同上
* GROUP BY项
* AGGREGATE BY项，后面跟`elasticsearch`中的聚类条件
