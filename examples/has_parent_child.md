elasticsearch中的`has_parent`,`has_child`查询(`parent_id`暂未添加)

## has_parent示例

SQL：

```sql
select * from fruit where has_parent(vegetable,weight between 100 and 400);
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "has_parent" : {
      "query" : {
        "range" : {
          "weight" : {
            "from" : "100",
            "to" : "400",
            "include_lower" : true,
            "include_upper" : true,
            "boost" : 1.0
          }
        }
      },
      "parent_type" : "vegetable",
      "score" : true,
      "ignore_unmapped" : false,
      "boost" : 1.0
    }
  },
  "_source" : {
    "includes" : [ ],
    "excludes" : [ ]
  },
  "track_total_hits" : -1
}
```

## has_child示例

SQL：

```sql
select * from fruit where has_child(apple,price in (10,20,30));
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "has_child" : {
      "query" : {
        "terms" : {
          "price" : [ "10", "20", "30" ],
          "boost" : 1.0
        }
      },
      "type" : "apple",
      "score_mode" : "avg",
      "min_children" : 1,
      "max_children" : 2147483647,
      "ignore_unmapped" : false,
      "boost" : 1.0
    }
  },
  "_source" : {
    "includes" : [ ],
    "excludes" : [ ]
  },
  "track_total_hits" : -1
}
```

