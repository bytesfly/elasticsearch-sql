

## select示例1

SQL：

```sql
select productName from apple;
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
    "includes" : [ "productName" ],
    "excludes" : [ ]
  },
  "track_total_hits" : -1
}
```

## select示例2

SQL：

```sql
select * from apple where minPrice > 5000;
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "range" : {
      "minPrice" : {
        "from" : "5000",
        "to" : null,
        "include_lower" : false,
        "include_upper" : true,
        "boost" : 1.0
      }
    }
  },
  "_source" : {
    "includes" : [ ],
    "excludes" : [ ]
  },
  "track_total_hits" : -1
}
```

## select示例3

SQL：

```sql
select ^productName, minPrice from apple;
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
    "includes" : [ "minPrice" ],
    "excludes" : [ "productName" ]
  },
  "track_total_hits" : -1
}
```

## select示例4

SQL：

```sql
select ^product*,min* from apple;
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
    "includes" : [ "min*" ],
    "excludes" : [ "product*" ]
  },
  "track_total_hits" : -1
}
```

## exists

SQL：

```sql
select * from apple where provider is not null;
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "exists" : {
      "field" : "provider",
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

## match

SQL：

```sql
select * from apple where provider ~= '苹果';
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "match" : {
      "provider" : {
        "query" : "苹果",
        "operator" : "OR",
        "prefix_length" : 0,
        "max_expansions" : 50,
        "fuzzy_transpositions" : true,
        "lenient" : false,
        "zero_terms_query" : "NONE",
        "auto_generate_synonyms_phrase_query" : true,
        "boost" : 1.0
      }
    }
  },
  "_source" : {
    "includes" : [ ],
    "excludes" : [ ]
  },
  "track_total_hits" : -1
}
```

## not match

SQL：

```sql
select * from apple where provider !~= '苹果';
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "bool" : {
      "must_not" : [ {
        "match" : {
          "provider" : {
            "query" : "苹果",
            "operator" : "OR",
            "prefix_length" : 0,
            "max_expansions" : 50,
            "fuzzy_transpositions" : true,
            "lenient" : false,
            "zero_terms_query" : "NONE",
            "auto_generate_synonyms_phrase_query" : true,
            "boost" : 1.0
          }
        }
      } ],
      "adjust_pure_negative" : true,
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

## match_phrase

SQL：

```sql
select * from apple where provider ~== '苹果';
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "match_phrase" : {
      "provider" : {
        "query" : "苹果",
        "slop" : 0,
        "zero_terms_query" : "NONE",
        "boost" : 1.0
      }
    }
  },
  "_source" : {
    "includes" : [ ],
    "excludes" : [ ]
  },
  "track_total_hits" : -1
}
```

## not match_phrase

SQL：

```sql
select * from apple where provider !~== '苹果';
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "bool" : {
      "must_not" : [ {
        "match_phrase" : {
          "provider" : {
            "query" : "苹果",
            "slop" : 0,
            "zero_terms_query" : "NONE",
            "boost" : 1.0
          }
        }
      } ],
      "adjust_pure_negative" : true,
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

## match_phrase_prefix

SQL：

```sql
select * from apple where provider ~=== '苹果';
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "match_phrase_prefix" : {
      "provider" : {
        "query" : "苹果",
        "slop" : 0,
        "max_expansions" : 50,
        "zero_terms_query" : "NONE",
        "boost" : 1.0
      }
    }
  },
  "_source" : {
    "includes" : [ ],
    "excludes" : [ ]
  },
  "track_total_hits" : -1
}
```

## not match_phrase_prefix

SQL：

```sql
select * from apple where provider !~=== '苹果';
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "bool" : {
      "must_not" : [ {
        "match_phrase_prefix" : {
          "provider" : {
            "query" : "苹果",
            "slop" : 0,
            "max_expansions" : 50,
            "zero_terms_query" : "NONE",
            "boost" : 1.0
          }
        }
      } ],
      "adjust_pure_negative" : true,
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

## multi_match

SQL：

```sql
select * from student where (name,age) ~= 'hahah';
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
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
  },
  "_source" : {
    "includes" : [ ],
    "excludes" : [ ]
  },
  "track_total_hits" : -1
}
```

## query_string

SQL：

```sql
select * from apple where query by '苹果';
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "query_string" : {
      "query" : "苹果",
      "fields" : [ ],
      "type" : "best_fields",
      "default_operator" : "or",
      "max_determinized_states" : 10000,
      "enable_position_increments" : true,
      "fuzziness" : "AUTO",
      "fuzzy_prefix_length" : 0,
      "fuzzy_max_expansions" : 50,
      "phrase_slop" : 0,
      "escape" : false,
      "auto_generate_synonyms_phrase_query" : true,
      "fuzzy_transpositions" : true,
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

## term

SQL：

```sql
select * from apple where provider = '苹果';
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
  "track_total_hits" : -1
}
```

## not term

SQL：

```sql
select * from apple where provider != '苹果';
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "bool" : {
      "must_not" : [ {
        "term" : {
          "provider" : {
            "value" : "苹果",
            "boost" : 1.0
          }
        }
      } ],
      "adjust_pure_negative" : true,
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

## terms

SQL：

```sql
select * from apple where provider in ('苹果','apple');
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "terms" : {
      "provider" : [ "苹果", "apple" ],
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

## not terms

SQL：

```sql
select * from apple where provider not in ('苹果','apple');
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "bool" : {
      "must_not" : [ {
        "terms" : {
          "provider" : [ "苹果", "apple" ],
          "boost" : 1.0
        }
      } ],
      "adjust_pure_negative" : true,
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

## fuzzy

SQL：

```sql
select * from apple where provider fuzzy like 'ca';
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "fuzzy" : {
      "provider" : {
        "value" : "ca",
        "fuzziness" : "AUTO",
        "prefix_length" : 0,
        "max_expansions" : 50,
        "transpositions" : true,
        "boost" : 1.0
      }
    }
  },
  "_source" : {
    "includes" : [ ],
    "excludes" : [ ]
  },
  "track_total_hits" : -1
}
```

## prefix

SQL：

```sql
select * from apple where provider prefix like 'ca';
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "prefix" : {
      "provider" : {
        "value" : "ca",
        "boost" : 1.0
      }
    }
  },
  "_source" : {
    "includes" : [ ],
    "excludes" : [ ]
  },
  "track_total_hits" : -1
}
```

## wildcard

SQL：

```sql
select * from apple where provider wildcard like 'ki*y';
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "wildcard" : {
      "provider" : {
        "wildcard" : "ki*y",
        "boost" : 1.0
      }
    }
  },
  "_source" : {
    "includes" : [ ],
    "excludes" : [ ]
  },
  "track_total_hits" : -1
}
```

## regexp示例1

SQL：

```sql
select * from apple where provider like 'k.*y';
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "regexp" : {
      "provider" : {
        "value" : "k.*y",
        "flags_value" : 255,
        "max_determinized_states" : 10000,
        "boost" : 1.0
      }
    }
  },
  "_source" : {
    "includes" : [ ],
    "excludes" : [ ]
  },
  "track_total_hits" : -1
}
```

## regexp示例2

SQL：

```sql
select * from apple where provider regexp like 'k.*y';
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "regexp" : {
      "provider" : {
        "value" : "k.*y",
        "flags_value" : 255,
        "max_determinized_states" : 10000,
        "boost" : 1.0
      }
    }
  },
  "_source" : {
    "includes" : [ ],
    "excludes" : [ ]
  },
  "track_total_hits" : -1
}
```

## must not

SQL：

```sql
select * from aaa where (not provider ~=== 'apple') or provider !~= 'microsoft';
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "bool" : {
      "should" : [ {
        "bool" : {
          "must_not" : [ {
            "match_phrase_prefix" : {
              "provider" : {
                "query" : "apple",
                "slop" : 0,
                "max_expansions" : 50,
                "zero_terms_query" : "NONE",
                "boost" : 1.0
              }
            }
          } ],
          "adjust_pure_negative" : true,
          "boost" : 1.0
        }
      }, {
        "bool" : {
          "must_not" : [ {
            "match" : {
              "provider" : {
                "query" : "microsoft",
                "operator" : "OR",
                "prefix_length" : 0,
                "max_expansions" : 50,
                "fuzzy_transpositions" : true,
                "lenient" : false,
                "zero_terms_query" : "NONE",
                "auto_generate_synonyms_phrase_query" : true,
                "boost" : 1.0
              }
            }
          } ],
          "adjust_pure_negative" : true,
          "boost" : 1.0
        }
      } ],
      "adjust_pure_negative" : true,
      "minimum_should_match" : "1",
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

## range示例1

SQL：

```sql
select * from apple where minPrice ranged in (1,3];
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "range" : {
      "minPrice" : {
        "from" : "1",
        "to" : "3",
        "include_lower" : false,
        "include_upper" : true,
        "boost" : 1.0
      }
    }
  },
  "_source" : {
    "includes" : [ ],
    "excludes" : [ ]
  },
  "track_total_hits" : -1
}
```

## range示例2

SQL：

```sql
select * from apple where minPrice >1 and minPrice <=3;
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "bool" : {
      "must" : [ {
        "range" : {
          "minPrice" : {
            "from" : "1",
            "to" : null,
            "include_lower" : false,
            "include_upper" : true,
            "boost" : 1.0
          }
        }
      }, {
        "range" : {
          "minPrice" : {
            "from" : null,
            "to" : "3",
            "include_lower" : true,
            "include_upper" : true,
            "boost" : 1.0
          }
        }
      } ],
      "adjust_pure_negative" : true,
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

## bool

SQL：

```sql
select * from apple where (provider='苹果' and minPrice>=5000) or productCode=404;
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "bool" : {
      "should" : [ {
        "bool" : {
          "must" : [ {
            "term" : {
              "provider" : {
                "value" : "苹果",
                "boost" : 1.0
              }
            }
          }, {
            "range" : {
              "minPrice" : {
                "from" : "5000",
                "to" : null,
                "include_lower" : true,
                "include_upper" : true,
                "boost" : 1.0
              }
            }
          } ],
          "adjust_pure_negative" : true,
          "boost" : 1.0
        }
      }, {
        "term" : {
          "productCode" : {
            "value" : "404",
            "boost" : 1.0
          }
        }
      } ],
      "adjust_pure_negative" : true,
      "minimum_should_match" : "1",
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

