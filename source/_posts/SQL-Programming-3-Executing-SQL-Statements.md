---
title: 'SQL Programming(3): Executing SQL Statements'
categories: [编程, Qt]
date: 2018-08-01 13:02:48
tags: [Qt, SQL, 翻译]
---

  QSqlQuery 类为执行SQL语句和浏览查询结果上提供了交互接口。

  QSqlQueryModel类和QSqlTableModel在访问数据库上提供了更高水平的接口，这两个类将在下一部分进行说明。如果你不熟悉SQL，那么你可以直接查看下一部分[Using the SQL Model Class]()。

##  执行一条语句

  要执行一条SQL语句，你可以创建一个QSqlQuery 对象，然后调用_QSqlQuery:exec()_函数，如下：

```c++
 QSqlQuery query;
 query.exec("SELECT name, salary FROM employee WHERE salary > 50000");
```

  QSqlQuery 的构造函数接受一个可选的QSqlDatabase对象作为参数，用来指定要使用的数据库。在上述例子中，QSqlQuery 对象在构造时没有传参来指定数据库，所以会使用默认的数据库连接。如果发生了错误，exec()函数会返回false。之后可以使用_QSqlQuery:lastError()_函数来查看错误。

## 浏览查询结果

  QSqlQuery 类可以逐一访问结果集的每一条记录。在调用exec()函数后，QSqlQuery 的内部指针会指向第一条记录之前的位置。我们必须调用_QSqlQuery:next()_ 函数将指针指向第一条记录，之后通过反复调用next()函数来获取其他记录，直到next()函数返回false。以下是一个典型的循环来遍历所有记录:
```C++
while (query.next()) {
    QString name = query.value(0).toString();
    int salary = query.value(1).toInt();
    qDebug() << name << salary;
}
```

 _QSqlQuery:value()_函数返回当前记录的指定字段的值。字段从0开始进行索引。_QSqlQuery::value()_函数返回一个QVariant类对象，它支持各种C++和Qt的核心数据类型，例如int，QString，QByteArray。数据库中不同的类型会自动映射到Qt中最接近等效的类型。在代码中，通过调用_QVariant:toString()_ 和_QVariant:toInt()_函数将变体类型分别转换为QString类型和int类型。对Qt所支持的数据库类型的推荐概述，请参见此表。你可以使用_QSqlQuery::next()_, _QSqlQuery::previous()_, _QSqlQuery::first()_, _QSqlQuery::last()_, 和_QSqlQuery::seek()_函数对结果集进行反复查询。可以使用_QSqlQuery:at()_函数来获取当前行的索引，在数据库支持的情况下，可以使用_QSqlQuery:size()_函数来获取结果集的总行数。使用_QSQLDriver:hasFeature()_函数可以确定一个数据库是否支持给定的特性。在下面的例子中，如果数据库支持size特性，通过调用_QSqlQuery:size()_函数来获取结果集的记录数，不然，通过访问最后一条记录，利用记录的位置来总的记录数。
```C++
QSqlQuery query;
int numRows;
query.exec("SELECT name, salary FROM employee WHERE salary > 50000");
QSqlDatabase defaultDB = QSqlDatabase::database();
if (defaultDB.driver()->hasFeature(QSqlDriver::QuerySize)) {
	numRows = query.size();
} else {
 // this can be very slow
 query.last();
 numRows = query.at() + 1;
}
```

  如果你在遍历结果时只通过传递正整数值来调用函数next() seek()，那么，你可以在调用exec()函数前，调用_QSqlQuery::setForwardOnly(true)_。在针对大数据结果集进行操作时，这个简单的做法会显著的加快查询的速度。

## 插入，更新，删除记录


  QSqlQuery 类可以执行任意的SQL语句，不单单是select。下面的例子说明了如何使用insert 将一条记录插入数据表中：

```C++
QSqlQuery query;
query.exec("INSERT INTO employee (id, name, salary) "
 "VALUES (1001, 'Thad Beaumont', 65000)");
```

  如果你想要同时插入多条记录，那么将真实的数据和SQL语句分开是一种比较有效的做法。这就要用到占位符。Qt支持两种占位符语法：命名绑定和位置绑定。下面是命名绑定的例子：

```C++
 QSqlQuery query;
 query.prepare("INSERT INTO employee (id, name, salary) "
 "VALUES (:id, :name, :salary)");
 query.bindValue(":id", 1001);
 query.bindValue(":name", "Thad Beaumont");
 query.bindValue(":salary", 65000);
 query.exec();
```

位置绑定示例：

```C++
 QSqlQuery query;
 query.prepare("INSERT INTO employee (id, name, salary) "
 "VALUES (?, ?, ?)");
 query.addBindValue(1001);
 query.addBindValue("Thad Beaumont");
 query.addBindValue(65000);
 query.exec();
```

  以上两种语法在Qt支持的数据库中都是有效的。如果数据库本就支持上述语法，那么Qt就会简单的将语句转发给数据库系统进行执行；否则，Qt会通过预处理查询来模拟占位符语法。最后，数据库执行的真正语句可以通过 _QSqlQuery:executedQuery()_ 函数获得。需要插入多条记录时，只需要提前调用一次 _QSqlQuery:prepare()_ 函数。之后你可以在调用exec()函数之前，根据需要调用_bindValue()_ 或者 _addBindValue()_ 函数。除了性能，使用占位符的另外一个优点在于，你可以指定任意值而不用担心转义字符的问题。

  更新一条记录和插入记录类似：

```C++
QSqlQuery query;
query.exec("UPDATE employee SET salary = 70000 WHERE id = 1003");
```
  你同样可以使用命名绑定或者位置绑定将参数值和真值关联起来。

  最后，这是删除语句的例子：

```C++
QSqlQuery query;
query.exec("DELETE FROM employee WHERE id = 1007");
```

## 事务处理

  如果基础数据库引擎支持事务处理，那么 _QSqlDriver::hasFeature(QSqlDriver::Transactions)_ 函数的返回值为true。首先使用 _QSqlDatabase::transaction()_ 来启动事务，其次为要执行的SQL命令，最后调用 _QSqlDatabase::commit()_ 或者 _QSqlDatabase::rollback()_ 函数。当使用事务创建查询之前，首先要启动事务。

例子：

```C++
QSqlDatabase::database().transaction();
QSqlQuery query;
query.exec("SELECT id FROM employee WHERE name = 'Torild Halvorsen'");
if (query.next()) {
 int employeeId = query.value(0).toInt();
 query.exec("INSERT INTO project (id, name, ownerid) "
 "VALUES (201, 'Manhattan Project', "
 + QString::number(employeeId) + ')');
}
QSqlDatabase::database().commit();
```

  事务机制的使用是用来保证一个复杂操作的原子性（举个例子，查询一个外键和增加一条记录），或是提供一种消除中间复杂性变化的手段。

***

Qt SQL Programming  系列翻译

-  [SQL Programming(1): Overview](https://conner.work/2015/02/11/SQL-Programming-1-Overview/#more)
-  [SQL Programming (2): Connection to Databases](https://conner.work/2018/07/23/SQL%20Programming%20%EF%BC%882%EF%BC%89%EF%BC%9A%20Connectiong%20to%20Databases/)

***
