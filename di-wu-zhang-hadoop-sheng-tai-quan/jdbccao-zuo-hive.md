## 5.11 JDBC操作Hive

maven的pom.xml

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
         <modelVersion>4.0.0</modelVersion>
         <groupId>com.tianshouzhi</groupId>
         <artifactId>hadoop</artifactId>
         <version>0.0.1-SNAPSHOT</version>
         <dependencies>
                   <dependency>
                            <groupId>org.apache.hadoop</groupId>
                            <artifactId>hadoop-common</artifactId>
                            <version>2.6.0-cdh5.4.7</version>
                            <scope>provided</scope>
                   </dependency>
                   <dependency>
                            <groupId>org.apache.hadoop</groupId>
                            <artifactId>hadoop-hdfs</artifactId>
                            <version>2.6.0-cdh5.4.7</version>
                            <scope>provided</scope>
                   </dependency>
                   <dependency>
                            <groupId>org.apache.hadoop</groupId>
                            <artifactId>hadoop-mapreduce-client-app</artifactId>
                            <version>2.6.0-cdh5.4.7</version>
                            <scope>provided</scope>
                   </dependency>
                   <dependency>
                            <groupId>org.apache.hive</groupId>
                            <artifactId>hive-jdbc</artifactId>
                            <version>1.1.0-cdh5.4.7</version>
                   </dependency>
                   <dependency>
                            <groupId>jdk.tools</groupId>
                            <artifactId>jdk.tools</artifactId>
                            <version>1.7</version>
                            <scope>system</scope>
                            <systemPath>${JAVA_HOME}/lib/tools.jar</systemPath>
                   </dependency>
                   <dependency>
                            <groupId>junit</groupId>
                            <artifactId>junit</artifactId>
                            <version>4.9</version>
                   <!--           <scope>test</scope> -->
                   </dependency>
         </dependencies>
         <repositories>
                   <repository>
                            <id>cloudera</id>
                            <url>https://repository.cloudera.com/artifactory/cloudera-repos/</url>
                   </repository>
         </repositories>
</project>
```

**使用Java操作**

```
package hive;
 
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
 
public class HiveJavaAPI {
         private static String driverName = "org.apache.hive.jdbc.HiveDriver"; 
    private static String url = "jdbc:hive2://115.28.65.149:10000/default"; 
    private static String user = "hive"; 
    private static String password = "hive"; 
    private static String sql = ""; 
    private static ResultSet res; 
 
    public static void main(String[] args) { 
        Connection conn = null; 
        Statement stmt = null; 
        try { 
            conn = getConn(); 
            System.out.println(conn);
            stmt = conn.createStatement(); 
//            String tableName="testHive";
            // 第一步:存在就先删除 
            String tableName = dropTable(stmt); 
 
            // 第二步:不存在就创建 
            createTable(stmt, tableName);
 
            // 第三步:查看创建的表 
            showTables(stmt, tableName); 
 
            // 执行describe table操作 
            describeTables(stmt, tableName); 
  
            // 执行load data into table操作 
//            loadData(stmt, tableName); 
 
            // 执行 select * query 操作 
            selectData(stmt, tableName); 
 
            // 执行 regular hive query 统计操作 
            countData(stmt, tableName); 
 
        } catch (ClassNotFoundException e) { 
            e.printStackTrace(); 
            System.exit(1); 
        } catch (SQLException e) { 
            e.printStackTrace(); 
            System.exit(1); 
        } finally { 
            try { 
                if (conn != null) { 
                    conn.close(); 
                    conn = null; 
                } 
                if (stmt != null) { 
                    stmt.close(); 
                    stmt = null; 
                } 
            } catch (SQLException e) { 
                e.printStackTrace(); 
            } 
        } 
    } 
 
    private static void countData(Statement stmt, String tableName) 
            throws SQLException { 
        sql = "select count(1) from " + tableName; 
        System.out.println("Running:" + sql); 
        res = stmt.executeQuery(sql); 
        System.out.println("执行“regular hive query”运行结果:"); 
        while (res.next()) { 
            System.out.println("count ------>" + res.getString(1)); 
        } 
    } 
 
    private static void selectData(Statement stmt, String tableName) 
            throws SQLException { 
        sql = "select * from " + tableName; 
        System.out.println("Running:" + sql); 
        res = stmt.executeQuery(sql); 
        System.out.println("执行 select * query 运行结果:"); 
        while (res.next()) { 
            System.out.println(res.getInt(1) + "\t" + res.getString(2)); 
        } 
    } 
 
    private static void loadData(Statement stmt, String tableName) 
            throws SQLException { 
        String filepath = "/tmp/root/stu.txt"; 
        sql = "load data local inpath '" + filepath + "' into table " 
                + tableName; 
        System.out.println("Running:" + sql); 
        stmt.execute(sql); 
    } 
 
    private static void describeTables(Statement stmt, String tableName) 
            throws SQLException { 
        sql = "describe " + tableName; 
        System.out.println("Running:" + sql); 
        res = stmt.executeQuery(sql); 
        System.out.println("执行 describe table 运行结果:"); 
        while (res.next()) { 
            System.out.println(res.getString(1) + "\t" + res.getString(2)); 
        } 
    } 
 
    private static void showTables(Statement stmt, String tableName) 
            throws SQLException { 
        sql = "show tables '" + tableName + "'"; 
        System.out.println("Running:" + sql); 
        res = stmt.executeQuery(sql); 
        System.out.println("执行 show tables 运行结果:"); 
        if (res.next()) { 
            System.out.println(res.getString(1)); 
        } 
    } 
 
    private static void createTable(Statement stmt, String tableName) 
            throws SQLException { 
        sql = "create table " 
                + tableName 
                + " (key int, value string)  row format delimited fields terminated by '\t'"; 
        stmt.execute(sql); 
    } 
 
    private static String dropTable(Statement stmt) throws SQLException { 
        // 创建的表名 
        String tableName = "testHive"; 
        sql = "drop table " + tableName; 
        stmt.execute(sql); 
        return tableName; 
    } 
 
    private static Connection getConn() throws ClassNotFoundException, 
            SQLException { 
        Class.forName(driverName); 
        Connection conn = DriverManager.getConnection(url, user, password); 
        return conn; 
    } 
}
```



