
## Maven
```xml
<dependency>
    <groupId>io.github.iamazy.elasticsearch.dsl</groupId>
    <artifactId>elasticsearch-sql-all</artifactId>
    <version>7.15.2</version>
</dependency>
```
或者
```xml
<dependencies>
    <dependency>
        <groupId>io.github.iamazy.elasticsearch.dsl</groupId>
        <artifactId>elasticsearch-sql-core</artifactId>
        <version>7.15.2</version>
    </dependency>
    <dependency>
        <groupId>io.github.iamazy.elasticsearch.dsl</groupId>
        <artifactId>elasticsearch-sql-jdbc</artifactId>
        <version>7.15.2</version>
    </dependency>
</dependencies>
```

## Plugin(isql)

### Installing

Elasticsearch {7.x}
```
./bin/elasticsearch-plugin install https://github.com/iamazy/elasticsearch-sql/releases/download/{isql-version}/elasticsearch-sql-plugin-{elasticsearch-version}.zip
```

### Usage

#### 1. query dataset with sql
```
POST _isql
{
    "sql":"select * from fruit"
}
```
#### 2. parse sql into elasticsearch dsl
```
POST _isql/_explain
{
    "sql":"select * from fruit"
}
```
