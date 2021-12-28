

## JDBC驱动

Elasticsearch-SQL JDBC驱动：

```java
static final String JDBC_DRIVER = "io.github.iamazy.elasticsearch.dsl.jdbc.ElasticDriver";
```

Elasticsearch-SQL JDBC连接字符串：

```java
static final String DB_URL = "jdbc:es://localhost:9200/index_demo1?useSSL=true&mode=single";
```

其中:
- `useSSL`表示是否使用`https`协议,默认为`false`,`mode`表示集群模式,`single`是单节点,`cluster`是`集群多节点`,`cross_cluster`表示`跨集群`,目前暂不支持
- 如果有多个`ip:port`,可以用逗号`,`隔开，如果有多个索引，也可以用`,`隔开,如：

```java
static final String DB_URL = "jdbc:es://localhost:9200,localhost:9201/index_demo1,index_demo2?useSSL=true&mode=single";
```

查询语句中的表名必须包含在**连接字符串**索引列表之中。

## JDBC示例

### 连接
```java
private static void conn1(){
        Connection conn = null;
        Statement stmt = null;
        try{
            Class.forName(JDBC_DRIVER);
            conn = DriverManager.getConnection(DB_URL,USER,PASS);

            stmt = conn.createStatement();
            String sql;
            sql = "SELECT * FROM index_demo1 where name = 'iamazy' limit 10";
            ResultSet rs = stmt.executeQuery(sql);
            while(rs.next()){
                String id  = rs.getString("_id");
                String name = rs.getString("name");
                String url = rs.getString("list");
                System.out.print("ID: " + id);
                System.out.print(", 站点名称: " + name);
                System.out.print(", 站点 URL: " + url);
                System.out.print("\n");
            }
            rs.close();
            stmt.close();
            conn.close();
        } catch(Exception se){
            se.printStackTrace();
        }
        finally{
            try{
                if(stmt!=null) stmt.close();
            }catch(SQLException ignored){
            }
            try{
                if(conn!=null) conn.close();
            }catch(SQLException se){
                se.printStackTrace();
            }
        }
    }
```
### 增删改
```java
public static void insert1(){
        Connection conn = null;
        Statement ps = null;
        try{
            Class.forName(JDBC_DRIVER);
            conn = DriverManager.getConnection(DB_URL,USER,PASS);
            String sql = "insert into index_demo1(name,age,list,_id) values('aaa',11,[1,2,2,1],111)";
            ps = conn.createStatement();
            int rs = ps.executeUpdate(sql);
            System.out.println(rs);
            ps.close();
            conn.close();
        } catch(Exception se){
            se.printStackTrace();
        }
        finally{
            try{
                if(ps!=null) ps.close();
            }catch(SQLException ignored){
            }
            try{
                if(conn!=null) conn.close();
            }catch(SQLException se){
                se.printStackTrace();
            }
        }
    }
```

### prepareStatement查询

```java
public static void prepareStatement1(){
        Connection conn = null;
        PreparedStatement ps = null;
        try{
            Class.forName(JDBC_DRIVER);
            conn = DriverManager.getConnection(DB_URL,USER,PASS);
            String sql = "SELECT * FROM index_demo1 where name =?  and age = ? limit 10";
            ps = conn.prepareStatement(sql);
            ps.setString(1,"aaa");
            ps.setInt(2,111);
            ResultSet rs = ps.executeQuery();
            while(rs.next()){
                String id  = rs.getString("_id");
                String name = rs.getString("name");
                String url = rs.getString("list");
                System.out.print("ID: " + id);
                System.out.print(", 站点名称: " + name);
                System.out.print(", 站点 URL: " + url);
                System.out.print("\n");
            }
            rs.close();
            ps.close();
            conn.close();
        } catch(Exception se){
            se.printStackTrace();
        }
        finally{
            try{
                if(ps!=null) ps.close();
            }catch(SQLException ignored){
            }
            try{
                if(conn!=null) conn.close();
            }catch(SQLException se){
                se.printStackTrace();
            }
        }
    }
```

### prepareStatement增删改

```java
public static void insert2(){
        Connection conn = null;
        PreparedStatement ps = null;
        try{
            Class.forName(JDBC_DRIVER);
            conn = DriverManager.getConnection(DB_URL,USER,PASS);
            String sql = "insert into index_demo1(name,age,list,_id) values(?,?,?,?)";
            ps = conn.prepareStatement(sql);
            ps.setString(1,"bbb");
            ps.setInt(2,3231);
            ps.setObject(3, Arrays.asList(1,3,2));
            ps.setString(4,"8373");
            int rs = ps.executeUpdate();
            System.out.println(rs);
            ps.close();
            conn.close();
        } catch(Exception se){
            se.printStackTrace();
        }
        finally{
            try{
                if(ps!=null) ps.close();
            }catch(SQLException ignored){
            }
            try{
                if(conn!=null) conn.close();
            }catch(SQLException se){
                se.printStackTrace();
            }
        }
    }
```

### Scroll方式导数据

> [!WARNING]
> Scroll方式必须指定 `ResultSet.TYPE_SCROLL_INSENSITIVE`,`ResultSet.CONCUR_READ_ONLY`

```java
public static void scroll(){
        Connection conn = null;
        PreparedStatement ps = null;
        try{
            Class.forName(JDBC_DRIVER);
            conn = DriverManager.getConnection(DB_URL,USER,PASS);
            String sql = "SELECT * FROM index_demo1";
            ps = conn.prepareStatement(sql,ResultSet.TYPE_SCROLL_INSENSITIVE,ResultSet.CONCUR_READ_ONLY);
            ResultSet rs = ps.executeQuery();
            int i=0;
            while(rs.next()){
                i++;
                System.out.println(rs.getString("_id")+"--"+ i);
            }
            rs.close();
            ps.close();
            conn.close();
        } catch(Exception se){
            se.printStackTrace();
        }
        finally{
            try{
                if(ps!=null) ps.close();
            }catch(SQLException ignored){
            }
            try{
                if(conn!=null) conn.close();
            }catch(SQLException se){
                se.printStackTrace();
            }
        }
    }
```
