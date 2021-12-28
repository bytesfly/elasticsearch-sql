高亮通过在字段前添加`h!`标志

## 示例1

SQL：

```sql
select h!nickname, h!name, h!class, hobby from student where (h!name, age) ~= 'hahah' and h!ab = 'haliluya';
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "bool" : {
      "must" : [ {
        "multi_match" : {
          "query" : "hahah",
          "fields" : [ "age^1.0", "name^1.0" ],
          "type" : "best_fields",
          "operator" : "OR",
          "slop" : 0,
          "prefix_length" : 0,
          "max_expansions" : 50,
          "zero_terms_query" : "NONE",
          "auto_generate_synonyms_phrase_query" : true,
          "fuzzy_transpositions" : true,
          "boost" : 1.0
        }
      }, {
        "term" : {
          "ab" : {
            "value" : "haliluya",
            "boost" : 1.0
          }
        }
      } ],
      "adjust_pure_negative" : true,
      "boost" : 1.0
    }
  },
  "_source" : {
    "includes" : [ "nickname", "name", "class", "hobby" ],
    "excludes" : [ ]
  },
  "track_total_hits" : -1,
  "highlight" : {
    "pre_tags" : [ "<span style=\"color:red\">" ],
    "post_tags" : [ "</span>" ],
    "require_field_match" : false,
    "fields" : {
      "nickname" : {
        "fragment_size" : 500,
        "number_of_fragments" : 0
      },
      "name" : {
        "fragment_size" : 500,
        "number_of_fragments" : 0
      },
      "ab" : {
        "fragment_size" : 500,
        "number_of_fragments" : 0
      },
      "class" : {
        "fragment_size" : 500,
        "number_of_fragments" : 0
      }
    }
  }
}
```

