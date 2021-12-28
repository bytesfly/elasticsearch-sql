目前支持的Geo查询 
- geo_bounding_box 
- geo_polygon 
- geo_distance 
- geo_shape 
- geojson_shape 


## geo_bounding_box示例

SQL：

```sql
select * from student where location.coordinate between [30,70] and [20,50];
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "range" : {
      "location.coordinate" : {
        "from" : "[30,70]",
        "to" : "[20,50]",
        "include_lower" : true,
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

## geo_polygon示例

SQL：

```sql
select * from student where location.coordinate in [[30.132,50],[40,60],[50,212]];
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "terms" : {
      "location.coordinate" : [ "[[30.132,50],[40,60],[50,212]]" ],
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

## geo_distance示例

SQL：

```sql
select * from student where location.coordinate=[40,30] and distance='100km';
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "geo_distance" : {
      "location.coordinate" : [ 40.0, 30.0 ],
      "distance" : 100000.0,
      "distance_type" : "arc",
      "validation_method" : "STRICT",
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

## geo_shape示例1

SQL：

```sql
-- 语法: where [field] [relation] [coordinates] shaped as [shape]
select * from student where location.coordinate within [[10,10],[20,20]] shaped as envelope;

```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "geo_shape" : {
      "location.coordinate" : {
        "shape" : {
          "type" : "Envelope",
          "coordinates" : [ [ 10.0, 20.0 ], [ 20.0, 10.0 ] ]
        },
        "relation" : "within"
      },
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

## geo_shape示例2

SQL：

```sql
select * from student where location.coordinate contains [[[10,10],[20,20]],[[11,11],[21,21]]] shaped as multilinestring;
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "geo_shape" : {
      "location.coordinate" : {
        "shape" : {
          "type" : "MultiLineString",
          "coordinates" : [ [ [ 10.0, 10.0 ], [ 20.0, 20.0 ] ], [ [ 11.0, 11.0 ], [ 21.0, 21.0 ] ] ]
        },
        "relation" : "contains"
      },
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

## geojson_shape

SQL：

```sql
select * from student where location.coordinate disjoint `{"coordinates": [[-155.52, 19.61], [-156.22, 20.74], [-157.97, 21.46]], "type": "MultiPoint"}`;
```

DSL：

```json
{
  "from" : 0,
  "size" : 15,
  "query" : {
    "geo_shape" : {
      "location.coordinate" : {
        "shape" : {
          "type" : "MultiPoint",
          "coordinates" : [ [ -155.52, 19.61 ], [ -156.22, 20.74 ], [ -157.97, 21.46 ] ]
        },
        "relation" : "disjoint"
      },
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

