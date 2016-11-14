---
title : JDBC与数据库访问
---

[TOC]


#JDBC与数据库访问

##关系数据库和SQL

###数据库概述

数据库是一个有组织的数据集合，它由一个或多个表组成。每一个表中都存储了对一类对象的数据描述。
数据库管理系统（database management system，DBMS）以一种与数据库格式一致的方式，提供了存储和组织数据的机制。

当前最流行的数据库是关系型数据库，它是将数据表示为表的集合，通过建立简单表之间的关系来定义结构的一种数据库。
数据库中的表按照行和列的形式来存储信息。行表示关系型数据库中的记录，列表示数据属性。
比较著名的关系数据库管理系统有Oracle、Sybase、DB2、MySQL、Microsoft SQL Server等。 


###数据库系统

- 数据库系统由数据库、数据库管理软件和应用软件组成

![](fig/dbms.png)

- 一个应用程序可以访问多个数据库系统

![](fig/dbms2.png)


###SQL

SQL是一个国际化标准语言，几乎所有关系型数据库都用SQL语言执行数据查询和操纵。 

####理解SQL语句时要注意几点：

- SQL语言中的语句都是独立执行的，无上下文联系；
- 每条语句都有自己的主关键字，语句中可包含若干子句；
- SQL语句本身不区分大小写。为突出语句格式，下面例子中保留字采用大写。


####SQL基本命令

#####建表语句

- 格式：

~~~sql
CREATE TABLE table_name （column1 type [not] null,…）
~~~

- 功能：
在当前数据库中创建一张名为的table_name表格结构。

#####删除表

- 格式：

~~~sql
DROP  table_name
~~~

- 功能：
在当前数据库中删除名为table_name的表。

#####查询语句

- 格式：

~~~sql
SELECT col1，col2，...，coln FROM table_name [WHERE condition_expression]
~~~

- 功能：
从数据库表中检索满足条件的记录。WHERE子句是可选项，它可以包含<、 >、 <=、 >=、 =、<>和LIKE运算符。
LIKE运算符用于带有通配符百分号（%）和下划线（_）的模式匹配。

- 例：

~~~sql
SELECT * FROM account WHERE    accountNumber='1280316401' 
~~~

#####插入语句

- 格式：

~~~sql
INSERT  INTO table_name 	[(col1，col2，...，coln)] VALUES(v1，v2，...，vn)
~~~

- 功能：
在表table_name中插入一条记录，各列的值依次分别为v1、v2、…、vn等，若某列的列名未给，则值为NULL。

- 注意：
(1)如果所有的列名都未给，则在Values中必须依次给出所有列的值。
(2)给出的值的类型必须与对应的列的类型相一致。 

#####更新语句

- 格式：

~~~sql
UPDATE  table_name  SET  col1=v1 [，col2=v2，...，coln=vn][WHERE  condition_expression]
~~~

- 功能：
更新表table_name中满足条件的记录，使列col1的值为v1、列col2的值为v2、…、列coln的值为vn等。

- 注意：
如不给出条件，则更新表中所有记录。

- 例如
account表中，账号为“1280316401“的账户取款200元后应更新余额，使用语句如下：

~~~sql
UPDATE account SET accountAmount=accountAmount-200 
WHERE accountNumber =’1280316401’
~~~

#####删除语句

- 格式：

~~~sql
    DELETE  FROM  table_name 	
	         [WHERE  condition_expression]
~~~

- 功能：
删除表table_name中满足条件的记录。

- 特别注意：
如果不给出条件，则删除表中所有记录。

- 例如，
对account表中，账号为“1280316401“的账户进行销户处理，语句如下：

~~~sql
DELETE FORM account WHERE accountNumber=’1280316401’ 
~~~



##JDBC概述

- 类似于Microsoft的ODBC[^1]。
- JDBC是Java DataBase Connectivity的缩写，它是一种可用于执行SQL语句的Java API，由一组用 Java编写的类和接口组成。
[^1]: ODBC是OpenDatabaseConnectivity的英文简写。它是由Microsoft提出的为连接不同数据库而制定的一种接口标准，是用C语言实现的，标准应用程序数据接口。通过ODBC API，应用程序可以存取保存在多种不同数据库管理系统（DBMS）中的数据，而不论每个DBMS使用了何种数据存储格式和编程接口。几乎所有的数据库都支持这一标准。<br>ODBC有其不足之处，比如它并不容易使用，没有面向对象的特性等等。<br>ODBC的结构包括四个主要部分：应用程序接口、驱动器管理器、数据库驱动器和数据源。 

- Java程序使用JDBC与数据库进行通信，并用它操纵数据库中的数据。
- JDBC主要提供了跨平台的数据库访问方法，为数据库应用开发人员提供了一种标准的应用程序设计接口，使- 开发人员可以用纯Java语言编写完整的数据库应用程序。
- JDBC是一种规范，它让各数据库厂商为Java程序员提供标准的数据库访问类和接口，这样就使得独立于DBMS的Java应用程序的开发工具和产品成为可能。

1996年夏，Sun公司推出了Java数据库连接（Java Database Connectivity，JDBC）工具包的第一个版本。 


##使用JDBC访问数据库

###JDBC数据库驱动程序的功能是：

- 一面用底层协议与数据库服务器进行对话；
- 一面用JDBC API与用户程序进行对话。

为实现 “与平台无关”的特点，JDBC为我们提供了一个“驱动程序管理器”，它能动态维护数据库查询所需的所有驱动程序对象。 
用户可以从数据库供应商那里获得JDBC数据库驱动程序。 

![](fig/jdbc.png)

Java应用程序 
Java程序包括应用程序、Applet和Servlet，这些类型的程序都可以利用JDBC方法完成对数据库的访问和操作。

JDBC驱动程序管理器
JDBC驱动程序管理器能够动态地管理和维护数据库查询所需要的所有厂商或第三方所提供的驱动程序对象，实现Java任务与特定驱动程序的连接，从而体现JDBC与平台无关这一特点。 

驱动程序
这里的驱动程序一般由数据库厂商或第三方提供，它由JDBC方法调用，向特定数据库发送SQL请求，并为Java程序获取结果。 

数据库
这里的数据库是指Java程序需要的数据库以及数据库管理系统。 


####JDBC驱动程序类型
JDBC驱动程序按照连接方式的不同可以分为四种类型： 

#####JDBC-ODBC Bridge
将对JDBC的调用转化为ODBC的调用，要求本地机必须安装ODBC驱动程序，然后注册一个ODBC数据源名 。

使用JDBC－ODBC Bridge，JDBC调用最终转化为ODBC调用，应用程序可以通过选择适当的ODBC驱动程序来实现对多个厂商的数据库访问。 
这种方式也存在局限性。
JDBC-ODBC Bridge采用Native代码（C语言），因此，在使用时，所有的本地数据库都必须安装在一台计算机上，并被正确设置。
这种数据库连接有着相当的开销和复杂性，因为调用必须从JDBC到Bridge，再到ODBC，并再从ODBC到本地客户API，直到数据库。
这种驱动程序不容许Java Applet即时发送。
ODBC不能解决的问题，JDBC－ODBC Bridge也不能解决，比如：Bridge不能通过Internet来访问数据库。

![](fig/jdbc2.png)

#####JDBC-Native API Bridge
直接将用户的调用转化为对数据库客户端API的调用，要求本地机必须安装好特定的驱动程序，显然限制了应用程序对其它数据库的使用。

Native API Bridge驱动程序利用客户机上的本地代码库来与数据库进行直接通信。与JDBC-ODBC Bridge一样，这种驱动程序也存在着许多限制。由于它使用的是本地库，因此这些库就必须事先安装在客户机上。 

![](fig/jdbc3.png)

#####JDBC-MiddleWare
它是独立于数据库服务器的，它和一个中间件服务器通讯，由中间件负责与数据库通讯。 

这种类型的JDBC驱动程序是4中类型中最灵活的。这种驱动程序通常被用在三层网络解决方案中，并能够被发布到Internet上。这种类型的驱动程序是一种纯Java的驱动程序，它将JDBC调用转换为一种与DBMS独立的网络协议并与某种中间层连接，然后通过中间层，采用第一、第二或第四中驱动程序与数据库通信。这种驱动程序通常由一些与数据库产品无关的公司开发。 

![](fig/jdbc4.png)

#####Pure JDBC Driver
使用该类型的应用程序无需安装附加的软件，所有对数据库的操作都直接由JDBC驱动程序完成。

这种JDBC驱动程序也是一种纯Java的驱动程序，它通过本地协议直接与数据库引擎相连。有了合适的通信协议，这种驱动程序也能够应用于Internet。该类驱动程序相对于其他类型的驱动程序的优势在于它的性能，在它与数据库引擎和客户机之间没有本地代码层或中间层软件。 

![](fig/jdbc5.png)


###JDBC API

简单地说，JDBC主要完成下列三项任务： 

- 同一个数据库建立连接；
- 向数据库发送SQL语句；
- 处理数据库返回的结果。 

这些任务由JDBC API来完成。JDBC API 被描述成为一组抽象的Java接口。
这些接口都可能产生异常，如：ClassNotFoundException、SQLException异常，因而编写程序时必须对抛出的异常进行捕获。

![](fig/jdbcapi.png)

####主要接口

(1) 驱动程序管理器DrvierManager
用来加载驱动程序，管理应用程序和已注册的驱动程序的连接。

(2) 连接Connection       
封装了应用程序与数据库之间的连接信息。

(3) 驱动程序Driver
负责定位并访问数据库，建立数据库连接和处理所有与数据库的通讯。

(4) 语句Statement	
用来在数据库中执行一条SQL语句。

(5) 结果集ResultSet	
负责保存执行查询后返回的数据。

###编写JDBC程序一般步骤

import java.sql.*;

（1）加载驱动程序：

~~~java
Class.forName(driverClass); 	
~~~

DriverManager 类是 JDBC 的管理层，作用于用户程序和驱动程序之间。它跟踪可用的驱动程序，并在数据库和相应驱动程序之间建立连接。负责管理JDBC驱动程序。使用JDBC驱动程序之前，必须先将驱动程序加载并向DriverManager注册后才可以使用，同时提供方法来建立与数据库的连接。
加载 Driver 类，并且实现自动在 DriverManager 中注册，这一过程通常通过调用方法 Class.forName()来完成，这将显式地加载驱动程序类。 


为了要连接数据库，您必须要有相应数据库的JDBC驱动程序，并将驱动程序的.jar文件加入到Classpath的设置中。 
DriverManager类管理各种数据库驱动程序，建立新的数据库连接，以便Java应用程序能够使用正确的JDBC驱动程序。
加载Driver类，然后自动在DriverManager中注册的方式有两种： 
1) 通过调用方法Class.forName（）。这种方法将显式地加载驱动程序类。 
2) 通过将驱动程序添加到java.lang.System 的属性jdbc.drivers 中。 
注意：加载驱动程序的第二种方法需要持久的预设环境。如果对这一点不能保证，则调用方法Class.forName 显式地加载每个驱动程序就显得更为安全。 



~~~java
Statement to load a driver:
Class.forName("JDBCDriverClass");

A driver（实现了java.sql.Driver接口的类） is a class.  For example:

Database   Driver Class                                 Source
Access      sun.jdbc.odbc.JdbcOdbcDriver    Already in JDK
MySQL    com.mysql.jdbc.Driver                   Website
Oracle       oracle.jdbc.driver.OracleDriver   Website

                            The JDBC-ODBC driver for Access is bundled in JDK. 
                   MySQL driver class is in mysqljdbc.jar 
          Oracle driver class is in classes12.jar

To use the MySQL and Oracle drivers, you have to add mysqljdbc.jar and classes12.jar in the classpath using the following DOS command on Windows:
      classpath=%classpath%;c:\book\mysqljdbc.jar;c:\book\classes12.jar

Class.forName("sun.jdbc.odbc.JdbcOdbcDriver");
//加载驱动程序，这里是一个JDBC-ODBC桥，类型一
Connection con = DriverManager.getConnection("jdbc:odbc:userlist", "user", "");
// 表示是通过ODBC来连接数据库的，userlist为ODBC数据源名, user为此数据源的用户ID
//建立一个连接

~~~

（2）建立与数据库的连接：

~~~java
Connection con = DriverManager.getConnection(url);
~~~

使用DriverManager类中的静态方法

~~~java
Connection con= getConnection(databaseURL)
~~~

Connection实际上是一个接口，它负责维护Java应用程序与数据库之间的连接。
Connection 对象代表与数据库的连接。连接过程包括所执行的 SQL 语句和在该连接上所返回的结果。一个应用程序可与单个数据库有一个或多个连接，或者可与许多数据库有连接。

|Database| URL Pattern| 
|:---|:---|          
|Access  | jdbc:odbc:dataSource|
|MySQL   | jdbc:mysql://hostname/dbname |
|Oracle  | jdbc:oracle:thin:@hostname:port#:oracleDBSID|

For Access:

>Connection connection = DriverManager.getConnection("jdbc:odbc:ExampleMDBDataSource");

For MySQL:

>Connection connection = DriverManager.getConnection("jdbc:mysql://localhost/test");
 
For Oracle:

>Connection connection = DriverManager.getConnection
  ("jdbc:oracle:thin:@liang.armstrong.edu:1521:orcl",  "scott", "tiger");


与数据库建立连接的标准方法是调用方法：

~~~java
DriverManger.getConnection(String url)
DriverManger.getConnection(String url, Properties info)
DriverManger.getConnection(String url, String user, String password)
~~~

JDBC中URL字符串的准确形式随着数据库的不同而有所变化，其一般形式是：

~~~java
jdbc:<subprotocol>:<subname> 
~~~



（3）创建Statement对象：

~~~java
Statement stmt = con.createStatement();
~~~

>Statement statement = connection.createStatement();

Statement 对象用于将 SQL 语句发送到数据库中，并返回结果。

方  法 : ResultSet executeQuery(String sql)
说  明 : 执行SQL查询指令select并返回结果集

方  法 : int executeUpdate(String sql)
说  明 : 执行对数据库修改的SQL指令如insert、delete、update等

方  法 :void close()
说  明 :断开对数据库的连接

(1) 创建 Statement 对象
Statement stmt = con.createStatement();

(2) 使用 Statement 对象执行语句 
String sql = "select * from userlist where username='" + username + "'";
ResultSet rs = stmt.executeQuery(sql); 

(3) 语句完成 
语句在已执行且所有结果返回时，即认为已完成。对于返回一个结果集的 executeQuery 方法，在检索完 ResultSet 对象的所有行时该语句完成。对于方法 executeUpdate，当它执行时语句即完成。 

Connection con =DriverManager.getConnection(url);
 Statement statement = connection.createStatement(); //创建Statement对象

statement.executeUpdate
     ("create table Temp (col1 char(5), col2 char(5))");

ResultSet resultSet = statement.executeQuery
  ("select firstName, mi, lastName from Student where lastName  = 'Smith'");

执行SQL语句 
连接一旦建立，就可用来向它所涉及的数据库传送SQL语句。
JDBC提供了三个类，用于向数据库发送SQL语句。Connection接口中的三个方法可用于创建这些类的实例。 
(1) Statement，由方法createStatement所创建。Statement 对象用于发送简单的SQL语句。
(2) PreparedStatement，由方法prepareStatement所创建。PreparedStatement对象用于发送带有一个或多个输入参数的SQL语句。 
(3) CallableStatement，由方法prepareCall所创建。CallableStatement对象用于执行SQL存储程序——一组可通过名称来调用（就像函数的调用那样）的SQL语句。  



（4）得到查询结果集或者执行UPDATE等操作：

~~~java
ResultSet rs = stmt.executeQuery(quaryStatement);
~~~

检索结果

SQL语句发送以后，返回的结果通常存放在一个ResultSet类的对象中，ResultSet对象可以看作是一个表，这个表中包含由SQL返回的列名和相应的值，ResultSet对象中维持了一个指向当前行的指针，通过一系列的getXXX方法，可以检索当前行的各个列，并显示出来。 


（5）对结果集（Resultset）提取执行结果：
使用next()方法，依次访问ResultSet对象中包含的检索结果行。或者使用getXXX方法从当前行指定列中提取不同类型的数据。

ResultSet结果集一般是一个表，该表的当前行可以访问，当前行的初始位置是null，使用next()能够移动到下一行，可以使用各种getXXX 方法从当前行检索值。

~~~java
ResultSet rs = stmt.executeQuery(sql);
//列印结果集
while(rs.next())
{
	String ps = rs.getString("password");
	if (ps.equals(password)) {
		//验证通过
		ok=true;
		}
	}
~~~


（6）关闭数据库连接：

在对象使用完毕后，应当使用close( )方法解除与数据库的连接，并关闭数据库。 

~~~java
con.close();
~~~

##使用JDBC访问数据库的示例

~~~java
import java.sql.*;
public class SimpleJdbc {
  public static void main(String[] args)
      throws SQLException, ClassNotFoundException {
    // Load the JDBC driver
    Class.forName("com.mysql.jdbc.Driver");
    System.out.println("Driver loaded");
 
    // Establish a connection
    Connection connection = DriverManager.getConnection
      ("jdbc:mysql://localhost/test");
    System.out.println("Database connected");
 
    // Create a statement
    Statement statement = connection.createStatement();
 
    // Execute a statement
    ResultSet resultSet = statement.executeQuery
      ("select firstName, mi, lastName from Student where lastName "
        + " = 'Smith'");
 
    // Iterate through the result and print the student names
    while (resultSet.next())
      System.out.println(resultSet.getString(1) + "\t" +
        resultSet.getString(2) + "\t" + resultSet.getString(3));
 
    // Close the connection
    connection.close();
  }
}

~~~

##练习

1．试述JDBC驱动程序有哪几种。
2．SQL语言包括哪几种基本语句来完成数据库的基本操作？
3．JDBC的作用是什么？主要完成什么任务？
4．怎样加载JDBC驱动程序类？举例说明。
5．Statement接口的作用是什么？
6．executeQuery()方法的作用是什么？
7．简述编写JDBC程序的一般步骤。

---

本文档 Github ：
https://github.com/bushehui/Java_tutorial












