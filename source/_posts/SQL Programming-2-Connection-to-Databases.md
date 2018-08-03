---
title: 'SQL Programming (2): Connection to Databases'
date: 2018-07-23 20:57:47
tags: [Qt, SQL, 翻译]
categories: 
- [编程, Qt]
- [Qt SQL Programming  系列翻译]
---

使用 QSqlQuery 或者 QSqlQueryModel 可以访问数据库，创建并打开一个或多个数据库连接。数据库连接通常是使用连接名而不是数据库名来区分彼此。你可以针对一个数据库创建多个连接。 QSqlDatabase 类创建数据库连接时，如果没有指定连接名，那么就为默认连接。当调用 QSqlQuery 或者 QSqlQueryModel 的成员函数（有一个参数为数据库连接名）时，如果未传递数据库的连接名称， 那么默认的数据库连接就会被使用。当你的应用程序只需要一个数据库连接时，那么创建一个默认连接无疑是方便的。

​	 **注意**：创建一个数据库连接和打开是不同的。创建一个连接涉及到实例化 QSqlDatabase 类，当打开它时，连接才会启用。下面的代码片段演示了如何创建一个默认的数据库连接，然后打开它：

```c++
 QSqlDatabase db = QSqlDatabase::addDatabase("QMYSQL");
 db.setHostName("bigblue");
 db.setDatabaseName("flightdb");
 db.setUserName("acarlson");
 db.setPassword("1uTbSbAs");
 bool ok = db.open();
```

​	  第一行，创建一个数据库连接对象，最后一行是打开数据库连接，之后使用。在此期间，我们初始化了一些连接信息，包括数据库名，主机名，用户名和密码。上述例子中，我们连接了一个 bigblue 主机上的名为 flightdb 的MySQL数据库。函数 _addDatabase()_ 的参数 "QMYSQL"指定了连接使用的数据库驱动类型。Qt所包含的数据库驱动集都列于支持的数据库驱动表中。 在上述代码段中，由于我们没有给 _addDatabase()_ 函数的第二个参数连接名传递参数，所以，上述的连接会成为默认连接。举个例子，接下来我们创建了两个MySQL数据的连接，并分别命名为"first" 和"second"：

```c++
QSqlDatabase firstDB = QSqlDatabase::addDatabase("QMYSQL", "first");
QSqlDatabase secondDB = QSqlDatabase::addDatabase("QMYSQL", "second");
```

​	 在上述两个连接的信息初始化之后，针对每个连接调用open函数来建立真正的连接。如果调用open函数失败，那么返回值为false。这样的话，我们可以调用 _QSqlDatabase::lastError()_ 函数来获取错误信息。一旦一个连接建立之后，我们就可以在任何地方调用 _QSqlDatabase::database()_ 静态函数来获取这个数据库连接的指针。如果我们没有指定连接名，那么我们将会得到默认连接的指针。

​	举个例子：

```c++
 QSqlDatabase defaultDB = QSqlDatabase::database();
 QSqlDatabase firstDB = QSqlDatabase::database("first");
 QSqlDatabase secondDB = QSqlDatabase::database("second");
```

​	如果要移除一个数据库连接，首先调用 _QSqlDatabase::close()_ 来关闭数据库，之后使用静态函数 _QSqlDatabase::removeDatabase()_ 来移除。



***
Qt SQL Programming  系列翻译

- {% post_link "SQL-Programming-1-Overview" "SQL Programming (1): Overview" %}
- {% post_link "SQL-Programming-3-Executing-SQL-Statements" "SQL Programming (3): Executing SQL Statements" %}
- {% post_link "SQL Programming-4-Using-the-SQL-Model-Classes" "SQL Programming (4): Using the SQL Model Classes" %}
- {% post_link "SQL Programming-5-Presenting-Data-in-a-Table-View" "SQL Programming (5): Presenting Data in a Table View" %}
- {% post_link "SQL Programming-6-Creating-Data-Aware-Forms" "SQL Programming (6): Creating Data Aware Forms" %}

***
