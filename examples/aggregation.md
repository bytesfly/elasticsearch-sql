分组(或者分桶)聚合查询

## group by示例1

SQL：

```sql
select max(age), count(name), max(height) from student group by name, age, height;
```

DSL：

```json
{
  "from" : 0,
  "size" : 0,
  "query" : {
    "match_all" : {
      "boost" : 1.0
    }
  },
  "_source" : {
    "includes" : [ ],
    "excludes" : [ ]
  },
  "track_total_hits" : -1,
  "aggregations" : {
    "count_name" : {
      "value_count" : {
        "field" : "name"
      }
    },
    "terms_name" : {
      "terms" : {
        "field" : "name",
        "size" : 5000,
        "min_doc_count" : 1,
        "shard_min_doc_count" : 0,
        "show_term_doc_count_error" : false,
        "order" : [ {
          "_count" : "desc"
        }, {
          "_key" : "asc"
        } ]
      },
      "aggregations" : {
        "max_age" : {
          "max" : {
            "field" : "age"
          }
        },
        "terms_age" : {
          "terms" : {
            "field" : "age",
            "size" : 5000,
            "min_doc_count" : 1,
            "shard_min_doc_count" : 0,
            "show_term_doc_count_error" : false,
            "order" : [ {
              "_count" : "desc"
            }, {
              "_key" : "asc"
            } ]
          },
          "aggregations" : {
            "terms_height" : {
              "terms" : {
                "field" : "height",
                "size" : 5000,
                "min_doc_count" : 1,
                "shard_min_doc_count" : 0,
                "show_term_doc_count_error" : false,
                "order" : [ {
                  "_count" : "desc"
                }, {
                  "_key" : "asc"
                } ]
              }
            },
            "max_height" : {
              "max" : {
                "field" : "height"
              }
            }
          }
        }
      }
    }
  }
}
```

## group by示例2

SQL：

```sql
select max(age) from student group by class;
```

DSL：

```json
{
  "from" : 0,
  "size" : 0,
  "query" : {
    "match_all" : {
      "boost" : 1.0
    }
  },
  "_source" : {
    "includes" : [ ],
    "excludes" : [ ]
  },
  "track_total_hits" : -1,
  "aggregations" : {
    "terms_class" : {
      "terms" : {
        "field" : "class",
        "size" : 5000,
        "min_doc_count" : 1,
        "shard_min_doc_count" : 0,
        "show_term_doc_count_error" : false,
        "order" : [ {
          "_count" : "desc"
        }, {
          "_key" : "asc"
        } ]
      }
    },
    "max_age" : {
      "max" : {
        "field" : "age"
      }
    }
  }
}
```

## group by示例3

SQL：

```sql
select count(*) from student group by class;
```

DSL：

```json
{
  "from" : 0,
  "size" : 0,
  "query" : {
    "match_all" : {
      "boost" : 1.0
    }
  },
  "_source" : {
    "includes" : [ ],
    "excludes" : [ ]
  },
  "track_total_hits" : -1,
  "aggregations" : {
    "terms_class" : {
      "terms" : {
        "field" : "class",
        "size" : 5000,
        "min_doc_count" : 1,
        "shard_min_doc_count" : 0,
        "show_term_doc_count_error" : false,
        "order" : [ {
          "_count" : "desc"
        }, {
          "_key" : "asc"
        } ]
      }
    },
    "count_id" : {
      "value_count" : {
        "field" : "_id"
      }
    }
  }
}
```

## group by示例4

SQL：

```sql
select count(distinct name) from student group by class;
```

DSL：

```json
{
  "from" : 0,
  "size" : 0,
  "query" : {
    "match_all" : {
      "boost" : 1.0
    }
  },
  "_source" : {
    "includes" : [ ],
    "excludes" : [ ]
  },
  "track_total_hits" : -1,
  "aggregations" : {
    "terms_class" : {
      "terms" : {
        "field" : "class",
        "size" : 5000,
        "min_doc_count" : 1,
        "shard_min_doc_count" : 0,
        "show_term_doc_count_error" : false,
        "order" : [ {
          "_count" : "desc"
        }, {
          "_key" : "asc"
        } ]
      }
    },
    "cardinality_name" : {
      "cardinality" : {
        "field" : "name"
      }
    }
  }
}
```

## group by示例5

SQL：

```sql
select count(*), count(distinct name), max(age) from student where gender = '男';
```

DSL：

```json
{
  "from" : 0,
  "size" : 0,
  "query" : {
    "term" : {
      "gender" : {
        "value" : "男",
        "boost" : 1.0
      }
    }
  },
  "_source" : {
    "includes" : [ ],
    "excludes" : [ ]
  },
  "track_total_hits" : -1,
  "aggregations" : {
    "count_id" : {
      "value_count" : {
        "field" : "_id"
      }
    },
    "cardinality_name" : {
      "cardinality" : {
        "field" : "name"
      }
    },
    "max_age" : {
      "max" : {
        "field" : "age"
      }
    }
  }
}
```

## aggregate_sum

SQL：

```sql
select * from apple where provider = '苹果' aggregate by sum(price);
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "term" : {
      "provider" : {
        "value" : "苹果",
        "boost" : 1.0
      }
    }
  },
  "_source" : {
    "includes" : [ ],
    "excludes" : [ ]
  },
  "track_total_hits" : -1,
  "aggregations" : {
    "sum" : {
      "sum" : {
        "field" : "price"
      }
    }
  }
}
```

## aggregate_max

SQL：

```sql
select * from apple where provider = '苹果' aggregate by max(price);
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "term" : {
      "provider" : {
        "value" : "苹果",
        "boost" : 1.0
      }
    }
  },
  "_source" : {
    "includes" : [ ],
    "excludes" : [ ]
  },
  "track_total_hits" : -1,
  "aggregations" : {
    "price" : {
      "max" : { }
    }
  }
}
```

## aggregate_min

SQL：

```sql
select * from apple where provider = '苹果' aggregate by min(price);
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "term" : {
      "provider" : {
        "value" : "苹果",
        "boost" : 1.0
      }
    }
  },
  "_source" : {
    "includes" : [ ],
    "excludes" : [ ]
  },
  "track_total_hits" : -1,
  "aggregations" : {
    "min" : {
      "min" : {
        "field" : "price"
      }
    }
  }
}
```

## aggregate_cardinality

SQL：

```sql
select * from apple aggregate by cardinality(productName);
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
  "track_total_hits" : -1,
  "aggregations" : {
    "productName_cardinality" : {
      "cardinality" : {
        "field" : "productName"
      }
    }
  }
}
```

## aggregate_terms

SQL：

```sql
select * from apple aggregate by terms(productName,10);
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
  "track_total_hits" : -1,
  "aggregations" : {
    "productName_terms" : {
      "terms" : {
        "field" : "productName",
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
    }
  }
}
```

## aggregate_top_hits

SQL：

```sql
select * from apple aggregate by top_hits(10);
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
  "track_total_hits" : -1,
  "aggregations" : {
    "top_hits" : {
      "top_hits" : {
        "from" : 0,
        "size" : 10,
        "version" : false,
        "seq_no_primary_term" : false,
        "explain" : false
      }
    }
  }
}
```

## sub-aggs

SQL：

```sql
select * from apple aggregate by terms(productName,10)>(terms(provider,10),cardinality(provider));
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
  "track_total_hits" : -1,
  "aggregations" : {
    "productName_terms" : {
      "terms" : {
        "field" : "productName",
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
        "provider_terms" : {
          "terms" : {
            "field" : "provider",
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
        "provider_cardinality" : {
          "cardinality" : {
            "field" : "provider"
          }
        }
      }
    }
  }
}
```

## nested

SQL：

```sql
select * from apple aggregate by [apple,terms(apple.color,2)];
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
  "track_total_hits" : -1,
  "aggregations" : {
    "nested_apple" : {
      "nested" : {
        "path" : "apple"
      },
      "aggregations" : {
        "apple.color_terms" : {
          "terms" : {
            "field" : "apple.color",
            "size" : 2,
            "shard_size" : 4,
            "min_doc_count" : 1,
            "shard_min_doc_count" : 1,
            "show_term_doc_count_error" : false,
            "order" : [ {
              "_count" : "desc"
            }, {
              "_key" : "asc"
            } ]
          }
        }
      }
    }
  }
}
```

