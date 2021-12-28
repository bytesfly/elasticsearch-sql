

## function_score

SQL：

```sql
select * from student where h!age> 10 and h!weight between 80 and 90 and color = 'red' func_score high > 160 && name ='小明' aggregate by terms(name,10),terms(age,10)>(cardinality(clazz));
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "function_score" : {
      "query" : {
        "bool" : {
          "must" : [ {
            "range" : {
              "age" : {
                "from" : "10",
                "to" : null,
                "include_lower" : false,
                "include_upper" : true,
                "boost" : 1.0
              }
            }
          }, {
            "range" : {
              "weight" : {
                "from" : "80",
                "to" : "90",
                "include_lower" : true,
                "include_upper" : true,
                "boost" : 1.0
              }
            }
          }, {
            "term" : {
              "color" : {
                "value" : "red",
                "boost" : 1.0
              }
            }
          } ],
          "adjust_pure_negative" : true,
          "boost" : 1.0
        }
      },
      "functions" : [ {
        "filter" : {
          "range" : {
            "high" : {
              "from" : "160",
              "to" : null,
              "include_lower" : false,
              "include_upper" : true,
              "boost" : 1.0
            }
          }
        },
        "weight" : 10.0
      }, {
        "filter" : {
          "term" : {
            "name" : {
              "value" : "小明",
              "boost" : 1.0
            }
          }
        },
        "weight" : 5.0
      } ],
      "score_mode" : "multiply",
      "max_boost" : 3.4028235E38,
      "boost" : 1.0
    }
  },
  "_source" : {
    "includes" : [ ],
    "excludes" : [ ]
  },
  "track_total_hits" : -1,
  "aggregations" : {
    "name_terms" : {
      "terms" : {
        "field" : "name",
        "size" : 10,
        "shard_size" : 20,
        "min_doc_count" : 1,
        "shard_min_doc_count" : 1,
        "show_term_doc_count_error" : false,
        "order" : [ {
          "_count" : "desc"
        }, {
          "_key" : "asc"
        } ]
      }
    },
    "age_terms" : {
      "terms" : {
        "field" : "age",
        "size" : 10,
        "shard_size" : 20,
        "min_doc_count" : 1,
        "shard_min_doc_count" : 1,
        "show_term_doc_count_error" : false,
        "order" : [ {
          "_count" : "desc"
        }, {
          "_key" : "asc"
        } ]
      },
      "aggregations" : {
        "clazz_cardinality" : {
          "cardinality" : {
            "field" : "clazz"
          }
        }
      }
    }
  },
  "highlight" : {
    "pre_tags" : [ "<span style=\"color:red\">" ],
    "post_tags" : [ "</span>" ],
    "require_field_match" : false,
    "fields" : {
      "weight" : {
        "fragment_size" : 500,
        "number_of_fragments" : 0
      },
      "age" : {
        "fragment_size" : 500,
        "number_of_fragments" : 0
      }
    }
  }
}
```

## dis_max

SQL：

```sql
select * from student dis_max age>11 || name='小米宁' and ( height>1.6 or fruit ='apple') || firstName is not null and tie_breaker=0.7;
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "dis_max" : {
      "tie_breaker" : 0.7,
      "queries" : [ {
        "range" : {
          "age" : {
            "from" : "11",
            "to" : null,
            "include_lower" : false,
            "include_upper" : true,
            "boost" : 1.0
          }
        }
      }, {
        "bool" : {
          "must" : [ {
            "term" : {
              "name" : {
                "value" : "小米宁",
                "boost" : 1.0
              }
            }
          } ],
          "should" : [ {
            "range" : {
              "height" : {
                "from" : "1.6",
                "to" : null,
                "include_lower" : false,
                "include_upper" : true,
                "boost" : 1.0
              }
            }
          }, {
            "term" : {
              "fruit" : {
                "value" : "apple",
                "boost" : 1.0
              }
            }
          } ],
          "adjust_pure_negative" : true,
          "minimum_should_match" : "1",
          "boost" : 1.0
        }
      }, {
        "exists" : {
          "field" : "firstName",
          "boost" : 1.0
        }
      } ],
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

