# JDBC



## DAY 1

### 内容

1. JDBC基本概念

2. 快速入门

3. JDBC中各个接口和类详解



#### JDBC:

1. * 概念：Java DataBse Connentivity       Java 数据库连接， Java语言操作数据库。
   * 本质：一套操作所有关系型数据库的规则，即接口。各个数据库厂商去实现这套接口，提供数据库驱动 Jar 包。我们使用这套接口（JDBC）编程，真正执行的代码是驱动 Jar 包中的实现类。



2. * 快速入门
     * 步骤：
       
       1. 导入驱动 Jar 包（可使用maven导入）
       2. 注册驱动
       3. 获取数据库的连接对象 Connection
       4. 定义SQL语句
       5. 获取执行SQL语句的对象 Statement
       6. 执行SQL，接受返回结果
       7. 处理结果
       8. 释放资源
       
     * 代码实现：
     
       ```java
       package jdbc;
       
       import java.sql.DriverManager;
       import java.sql.Statement;
       import java.sql.Connection;
       
       public class JdbcDemo1 {
           public static void main(String[] args) throws Exception {
               //1.导入Jar包
               //2.注册驱动
               Class.forName("com.mysql.jdbc.Driver");
               //3.获取数据库连接对象
               Connection conn = 		       								                              	 	 	  DriverManager.getConnection("jdbc:mysql://localhost:3306/school", "root",        	 			 "tyx19970802");
               //4.定义SQL语句
               String sql = "UPDATE students SET score = 888 WHERE id = 1;";
               //5.获取执行SQL语句的对象 Statement
               Statement stmt = conn.createStatement();
               //6.执行SQL，接受返回对象
               int count = stmt.executeUpdate(sql);
               //7.处理结果
               System.out.println(count);
               //8.释放资源
               stmt.close();
               conn.close();
           }
       }
       ```

3. 各个对象

   1. DviverManager：驱动管理对象

      * 功能

        1. 注册驱动：告诉程序该使用哪一个数据库驱动 Jar

           * Static void registerDriver(Driver driver)：注册于给定的驱动程序 DriverManager。 

           * 写代码使用：Class.forName("com.mysql.jdbc.Driver");

           * 通过查看源码发现：在com.mysql.jdbc.Driver类中存在静态代码块

           ```java
           static {
               try{
                   java.sql.DriverManager.registerDriver(new Driver());
               } catch (SQLException E) {
                   throw new RuntimeException("Can't register driver !");
               }
           }
           ```

              注意：mysql5之后的驱动 Jar包可以忽略注册驱动的步骤。

        2. 获取数据库连接

           * 方法：static Connection getConnection (String url, String user, String password)

           * 参数：

             * url：指定连接的路径

               * 语法：jdbc:mysql://ip地址（域名）：端口号/数据库名称

               * 例子：jdbc:mysql//localhost:3306/school

               * 注意：如果连接的是本机mysql服务器，并且mysql服务默认端口是3306，

                 ​			则 url 可简写成：jdbc:mysql:///数据库名称

             * user

             * password

   2. Connection：数据库连接对象

   3. Statement：执行SQL对象

   4. ResultSet：结果集对象

   5. PreparedStatement：执行SQL对象