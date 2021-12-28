`distinct`对应`elasticsearch`中的`collapse`字段折叠,只能有一个字段设置为`distinct`

## 示例1

SQL：

```sql
select distinct name from student;
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "match_all" : {
      "boost" : 1.0
    }
  },
  "_source" : {
    "includes" : [ "name" ],
    "excludes" : [ ]
  },
  "track_total_hits" : -1
}
```

