elasticsql理论上支持无限嵌套nested查询（虽然不推荐太大的嵌套层数）

语法：
> [nestedPath,expression]

比如mapping为
```json
"mappings": {
    "apple": {
        "properties": {
	    "productName": {
		"type": "string",
		"index": "not_analyzed"
	     },
	    "buyers": {
		"type": "nested",
		"properties": {
		    "buyerName": {
			"type": "string",
			"index": "not_analyzed"
		    },
		     "productPrice": {
			"type": "double"
		     },
                     "family":{
                        "type":"nested",
                        "properties":{
                            "son":{
                                "type":"keyword"
                            }
                        }
                    }
		}
	    }
        }
    }
}
```

示例SQL：

```sql
select * from apple where [buyers,buyers.buyerName='小明' and [buyers.family,buyers.family.son='小小明']];
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "nested" : {
      "query" : {
        "bool" : {
          "must" : [ {
            "term" : {
              "buyers.buyerName" : {
                "value" : "小明",
                "boost" : 1.0
              }
            }
          }, {
            "nested" : {
              "query" : {
                "term" : {
                  "buyers.family.son" : {
                    "value" : "小小明",
                    "boost" : 1.0
                  }
                }
              },
              "path" : "buyers.family",
              "ignore_unmapped" : false,
              "score_mode" : "avg",
              "boost" : 1.0
            }
          } ],
          "adjust_pure_negative" : true,
          "boost" : 1.0
        }
      },
      "path" : "buyers",
      "ignore_unmapped" : false,
      "score_mode" : "avg",
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
