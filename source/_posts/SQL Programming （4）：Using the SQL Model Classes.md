---

title: SQL Programming (4)：Using the SQL Model Classes

date: 2018-08-03 08:54:47

tags: [Qt, SQL, 翻译]

categories: 
- [编程, Qt]
- [Qt SQL Programming  系列翻译]
---

  

除了QSqlQuery类以外，Qt还提供了三个更复杂的访问数据库的类，QSqlQueryModel ,QSqlTableModel, QSqlRelationalTableModel.

- QSqlQueryModel : 一个基于任意SQL语句的只读模型

- QSqlTableModel：一个针对单表的可读写模型

- QSqlRelationalTableModel： 一个支持外键的QSQLTableModel的子类

  以上的三个类都源于QAbstractTableModel类（这个类继承自QAbstractItemModel），通过和一个Item View类结合（比如QListView、QTableView），使得从一个数据库中呈现数据的更加的简单。在Presenting Data in a Table View 章节，这一过程将会被详细的解释。

  使用这些类也能够让你的代码更加方便的更改为其他的数据源。举个例子，如果你开始使用的是QSqlTableModel类，之后觉得使用XML文档来替代之前使用的数据库来存储数据，那么这本质上只是一个数据模型替换为另一个的问题。

##  The SQL Query Model

  QSqlQueryModel 提供了基于一条SQL语句的只读模型。

  例子：

``` C++
 QSqlQueryModel model; 

 model.setQuery("SELECT * FROM employee"); 

 

 for (int i = 0; i < model.rowCount(); ++i) { 

  int id = model.record(i).value("id").toInt(); 

  QString name = model.record(i).value("name").toString(); 

  qDebug() << id << name; 

 }
```

  在调用了 _QSqlQueryModel::setQuery()_ 函数设置了查询语句后，你可以使用 _QSqlQueryModel::record(int)_ 来获取指定的查询记录。当然，你也可以使用   _QSqlQueryModel::data()_ 或者其他的继承自QAbstractItemModel的函数。

​    例外，_setQuery()_ 的一个重载函数支持两个参数，一个是一个QSqlQuery的对象，另外一个是要操作的结果集，也就是数据库。这可以让你更好的使用QSqlQuery的一些特性（比如，prepared queries）。

##  The SQL Table Model

​    QSqlTableModel提供了一个一次可以操作单个SQL表的可读写模型。

例子：

``` c++
 QSqlTableModel model;

 model.setTable("employee");

 model.setFilter("salary > 50000");

 model.setSort(2, Qt::DescendingOrder);

 model.select();

for (int i = 0; i < model.rowCount(); ++i) {

 QString name = model.record(i).value("name").toString();

 int salary = model.record(i).value("salary").toInt();

 qDebug() << name << salary;

}
```

   QSqlTableModel 针对 QSqlQuery，在单表的操作修改上提供了更好的交互方式。这样就使得开发者只需要很少的代码同时摆脱了SQL语法的束缚就能够完成数据操作。

​   使用 _QSqlTableModel:record()_ 函数可以获得表中的指定行，使用 _QSqlTableModel:setRecord()_ 来修改数据。举个例子，下面的代码会将每个员工的工资提高10%：

``` c++
for (int i = 0; i < model.rowCount(); ++i) {

 QSqlRecord record = model.record(i);

 double salary = record.value("salary").toInt();

 salary *= 1.1;

 record.setValue("salary", salary);

 model.setRecord(i, record);

}

 model.submitAll();
```

​    你也可以使用 _QSqlTableModel:data()_ 和 _QSqlTableModel:setData()_（继承自QAbstractItemModel）来获取数据。例如，下面是如何使用 _setData()_ 函数来更新一条记录：    

``` C++
 model.setData(model.index(row, column), 75000);

 model.submitAll();
```

​    以下是如何插入并填充一行数据：

``` C++
 model.insertRows(row, 1);

 model.setData(model.index(row, 0), 1013);

 model.setData(model.index(row, 1), "Peter Gordon");

 model.setData(model.index(row, 2), 68500);

 model.submitAll();
```

​    以下是如何删除五个连续的行数据：

``` C++
model.removeRows(row, 5);

model.submitAll();
```

​    函数 _QSqlTableModel:removeRows()_ 的第一个参数是第一个要被删除行的索引。    

​    当你修改完成一行记录后，应该调用 _QSqlTableModel::submitAll()_ 函数来保证所有的数据改变都被写入数据库。

​    至于在什么时候应该调用 _submitAll()_ 函数取决于表的编辑策略（edit strategy）。默认的编辑策略是 _QSqlTableModel::OnRowChange_，即在用户选取不同的行后，向数据库提交更改。其他的策略还有 _QSqlTableModel::OnManualSubmin()_（即缓存所有数据改变，直到调用 _submitAll()_ 函数），_QSqlTableModel::OnFieldChange()_（不缓存更改）。当 QSqlTableModel 和 View 类一起使用时，这些策略是非常有帮助的。

​    _QSqlTableModel::OnFieldChange()_ 意味着你永远不会显示的调用 _submitAll()_ 函数。尽管如此，这里面也存在这两个误区：

1. 没有任何缓存，意味着性能会显著下降；
2. 当你试图增加一个主键时，你来不及对它填充数据。

## The SQL Relational Table Model

​    QSqlRelationalTableModel 是对QSqlTableModel类的扩展，它提供了对外键的支持。一个外键是指一个表中的字段和另外一个表的主键存在一对一的映射关系。举个例子，如果book表中有一个名为 authorid 的字段指的是 author 表中的id字段（id字段是主键），那么 authorid 就是一个外键。

​        ![img](http://doc.qt.io/qt-5/images/noforeignkeys.png)

​        ![img](http://doc.qt.io/qt-5/images/foreignkeys.png)

​        第一副截图是在 QTableView 里使用了一个简单的QSqlTableModel。外键（city和Country）并没有被转化为可读值。第二幅截图使用的是QSqlRelationalTableModel，外键值被解析为可读的文本字符。

​    下面的代码片段演示了如何创建一个QSqlRelationalTableModel：

``` C++
model->setTable("employee");

model->setRelation(2, QSqlRelation("city", "id", "name"));

model->setRelation(3, QSqlRelation("country", "id", "name"));
```

​    详细用法，请查看QSqlRelationalTableModel的文档。



***
Qt SQL Programming  系列翻译

- {% post_link "SQL-Programming-1-Overview" "SQL Programming (1): Overview"%}
- {% post_link "SQL Programming （2）：Connection to Databases" "SQL Programming (2)：Connection to Databases"%}
- {% post_link "SQL-Programming-3-Executing-SQL-Statements" SQL Programming (3): Executing SQL Statements %}
***