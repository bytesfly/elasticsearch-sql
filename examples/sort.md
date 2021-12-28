不指定升降序则默认`desc`降序

## 示例1

SQL：

```sql
select * from apple order by minPrice, advicePrice asc;
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
    "includes" : [ ],
    "excludes" : [ ]
  },
  "sort" : [ {
    "minPrice" : {
      "order" : "desc",
      "mode" : "avg"
    }
  }, {
    "advicePrice" : {
      "order" : "asc",
      "mode" : "avg"
    }
  } ],
  "track_total_hits" : -1
}
```

