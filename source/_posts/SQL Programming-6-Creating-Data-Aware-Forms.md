---
title: 'SQL Programming (6): Creating Data Aware Forms'
date: 2018-08-03 13:10:48
tags: [Qt, SQL, 翻译]
categories: 
- [编程, Qt]
- [Qt SQL Programming  系列翻译]
---

​    使用上述SQL模块，一个数据库的内容可以使用其他的模型/视图模块来表示。对于一些应用程序来说，使用标准的item view 来显示信息已经足够了。然而，在基于用户数据的应用程序往往需要一个基于表格的用户交互界面，并且将数据库中指定行列的数据被填充到窗体的可编辑控件中去。我们可以使用QDataWidgetMapper类来创建上述的数据感知窗体，这个通用的模型/视图组件可以将一个模型中的数据和用户界面的指定控件相互映射。QDataWidgetMapper 类针对指定的数据库表，将表中的项按照行或者列进行数据映射。因此，将QDataWidgetMapper 类与一个SQL模型搭配使用 和将其与其他table模型搭配使用一样简单。

![img](http://doc.qt.io/qt-5/images/qdatawidgetmapper-simple.png)

​    上述 例子很好的说明了使用QDataWidgetMapper类和一些简单的输入控件来访问信息。

***
Qt SQL Programming  系列翻译

- {% post_link "SQL-Programming-1-Overview" "SQL Programming (1): Overview" %}
- {% post_link "SQL Programming-2-Connection-to-Databases" "SQL Programming (2): Connection to Databases" %}
- {% post_link "SQL-Programming-3-Executing-SQL-Statements" "SQL Programming (3): Executing SQL Statements" %}
- {% post_link "SQL Programming-4-Using-the-SQL-Model-Classes" "SQL Programming (4): Using the SQL Model Classes" %}
- {% post_link "SQL Programming-5-Presenting-Data-in-a-Table-View" "SQL Programming (5): Presenting Data in a Table View" %}

***