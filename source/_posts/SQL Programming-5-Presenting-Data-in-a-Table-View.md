---
title: 'SQL Programming (5): Presenting Data in a Table View'
date: 2018-08-03 13:05:48
tags: [Qt, SQL, 翻译]
categories: 
- [编程, Qt]
- [Qt SQL Programming  系列翻译]
---

​    QSqlQueryModel，QSqlTableModel， 和QSqlRelationalTableMode 类可以作为 Qt 视图类（例如QListView，QTableView 和QTreeView）的数据源。实践中，QTableView类是迄今为止最常见的选择，因为SQL结果集基本上是一个二维的数据结构。

![img](http://doc.qt.io/qt-5/images/relationaltable.png)

​    下面的例子创建了一个基于SQL数据模型的视图：

``` c++
 QTableView *view = new QTableView;

 view->setModel(model);

 view->show();
```

​    如果模型是可读写的（例如，QSqlTableModel），那么视图是允许用户修改字段的。你可以通过调用下面的代码来禁止掉用户修改

``` c++
view->setEditTriggers(QAbstractItemView::NoEditTriggers);
```

​    同一个数据模型可以作为多个视图的数据源。如果用户在其中一个视图中修改了数据模型，那么其他的视图会立刻做出相应的改变。The Table Model 例子说明了它是如何工作的。视图类显示了页眉顶部的标签栏。可以通过调用model中的setHeaderData()函数来修改标题文本。标题的标签文本默认为数据表的字段名。例如：

``` c++
 model->setHeaderData(0, Qt::Horizontal, QObject::tr("ID"));

 model->setHeaderData(1, Qt::Horizontal, QObject::tr("Name"));

 model->setHeaderData(2, Qt::Horizontal, QObject::tr("City"));

 model->setHeaderData(3, Qt::Horizontal, QObject::tr("Country"));
```

​    QTableView 在表的左侧有一个用数字标识行号的垂直表头。如果通过调用 _QSqlTableModel::insertRows()_ 函数来插入一行，那么新的一行将会标有 * 号，直到调用函数 _submitAll()_ ，或者用户移动到其他数据行时被自动提交（假设编辑策略为 QSqlTableModel::OnRowChange），*号才会消失。

![img](http://doc.qt.io/qt-5/images/insertrowinmodelview.png)

​    同样的，如果使用函数 _removeRows()_ 来移除行，被移除的行会被一个感叹号(!)标记直到更改被提交。视图中的项通过委托（delegate）实现。默认的委托类QItemDelegate可以处理最常见的数据类型（例如int，QString，QImage等）。当用户开始编辑视图中的项时，委托还负责提供编辑工具（例如，组合框）。你也可以通过子类化QAbstractItemDelegate 或者 QItemDelegate 类来实现你自己的委托类。更多的信息，可以查看[模型视图编程](qthelp://org.qt-project.qtwidgets.531/qtwidgets/model-view-programming.html)。

​    QSqlTableModel类针对单表操作进行了优化。如果需要一个可以操作任意结果集的可读写模型，你可以子类化QSqlTableModel，重新实现 _flags()_ 、_setData()_ 函数使其能够读写。下面的两个函数可以使一个查询模型的字段1、2可编辑：

``` c++
Qt::ItemFlags EditableSqlModel::flags(const QModelIndex &index) const

{

 Qt::ItemFlags flags = QSqlQueryModel::flags(index);

 if (index.column() == 1 || index.column() == 2)

    flags |= Qt::ItemIsEditable;

 return flags;

}

bool EditableSqlModel::setData(const QModelIndex &index, const QVariant &value, int /* role */)

{

 if (index.column() < 1 || index.column() > 2)

    return false;

 QModelIndex primaryKeyIndex = QSqlQueryModel::index(index.row(), 0);

 int id = data(primaryKeyIndex).toInt();

 clear();

 bool ok;

 if (index.column() == 1) {

    ok = setFirstName(id, value.toString());

 } else {

    ok = setLastName(id, value.toString());

 }

 refresh();

 return ok;

}
```

​       函数 _setFirstName()_ 的定义如下：

``` c++
bool EditableSqlModel::setFirstName(int personId, const QString &firstName)

{

 QSqlQuery query;

 query.prepare("update person set firstname = ? where id = ?");

 query.addBindValue(firstName);

 query.addBindValue(personId);

 return query.exec();

}
```

​    函数 _setLastName()_ 类似。完整源代码可以查看 例子[Query Model](qthelp://org.qt-project.qtsql.531/qtsql/qtsql-querymodel-example.html) 。

​    子类化一个模型类可以在很多方面进行定义：可以为项提供提示字符，改变背景色，提供计算后的值，为查看和编辑提供不同的值，处理空值等等。更多细节可以查看 [Model/View Programming](qthelp://org.qt-project.qtwidgets.531/qtwidgets/model-view-programming.html)以及[QAbstractItemView](qthelp://org.qt-project.qtwidgets.531/qtwidgets/qabstractitemview.html)参考文档。

​    如果你仅仅想要将外键转化为更友好的字符串，你可以使用QSqlRelationalTableModel类。你最好也使用 QSqlRelationalDelegate 类，它提供了组合框控件来更方便的边间外键。

![img](http://doc.qt.io/qt-5/images/relationaltable.png)

​    [Relational Table Model](qthelp://org.qt-project.qtsql.531/qtsql/qtsql-relationaltablemodel-example.html)例子说明了如何使用 QSqlRelationalTableModel 和 QSqlRelationalDelegate类为表提供外键支持。


***
Qt SQL Programming  系列翻译

- {% post_link "SQL-Programming-1-Overview" "SQL Programming (1): Overview" %}
- {% post_link "SQL Programming-2-Connection-to-Databases" "SQL Programming (2): Connection to Databases" %}
- {% post_link "SQL-Programming-3-Executing-SQL-Statements" "SQL Programming (3): Executing SQL Statements" %}
- {% post_link "SQL Programming-4-Using-the-SQL-Model-Classes" "SQL Programming (4): Using the SQL Model Classes" %}
- {% post_link "SQL Programming-6-Creating-Data-Aware-Forms" "SQL Programming (6): Creating Data Aware Forms" %}

***