---
title: 'SQL Programming(1): Overview'
comments: true
toc: true
categories:
  - Qt
date: 2015-02-11 22:46:14
tags:
  - Qt
  - SQL
  - 翻译
---

阅读本文，建议有一定基础的SQL知识，能够理解简单的SELECT, INSERT, UPDATE, 和DELETE语句。即便使用QSqlTableModel类不需要SQL知识便能够完成数据库的浏览和编辑功能，但是，还是强烈建议读者对SQL有一定的基础知识。[ Introduction to Database Systems (7th Ed.)](https://book.douban.com/subject/1768231/) 书中，涵盖了对SQL数据操作的标准语句。

## 主题：

- 数据库类（Database Classes）
- 链接数据库（connecting to Database）
  - 数据库驱动（SQL Database Drivers）
- 执行SQL语句（Executing SQL Statements）
  - Qt支持的数据库系统的数据类型（Data Types for Qt-supported Database Systems）
- 使用SQL模型类（using the SQL Model Classes）
- 在Table View中显示数据（Presenting Data in a Table View）
- 创建数据识别窗体（Creating Data-Aware Forms）


## 数据库类（Database Classes）
以下类提供对SQL数据库的访问。

- QSql                                       包含Qt SQL模块中所使用到的标示符（Enum，Flags）   
- QSqlDriverCreatorBase       SQL驱动工厂基类
- QSqlDriverCreator               模板类，提供了一种特定驱动类型的SQL驱动工厂
- QSqlDatabase                      代表一个到数据库的链接
- QSqlDriver                           访问特定SQL数据库的抽象基类
- QSqlError                            SQL数据库的错误信息
- QSqlField                            操作SQL数据库表和视图中的字段
- QSqlIndex                          描述和控制数据库索引的函数集
- QSqlQuery                          操作执行SQL语句的方法
- QSqlRecord                        封装了数据库的记录
- QSqlResult                          访问具体SQL数据库数据的抽象接口
- QSqlQueryModel                SQL结果集的只读数据模型
- QSqlRelationalTableModel   一个数据库表的可编辑数据模型，提供外键支持
- QSqlTableModel                一个数据库表的可编辑数据模型

SQL类分为3层：

### 驱动层：

包含 _QSqlDriver_ , _QSqlDriverCreator_ , _QSqlDriverCreatorBase_ , _QSqlDriverPlugin_ , 和 _QSqlResult_ 类。

这一层提供了具体的数据库和 SQL API层之间的底层桥梁。更多信息请参见SQL数据库驱动。

### SQL API层：

本层的类提供了对数据库的访问接口。通过使用QSqlDatabase类创建链接。QSqlQuery类实现数据库交互。 除了 QSqlDatabase 和 QSqlQuery类之外,本层还提供了 QSqlError, QSqlField, QSqlIndex, 和 QSqlRecord类。

### 用户界面层：

QSqlQueryModel, QSqlTableModel, 和QSqlRelationalTableModel类将数据库中的数据与数据呈现控件相连接。这些类设计成和Qt的模型/视图框架协同工作。

**注意** ：QCoreApplication 对象必须在使用这些类之前被实例化。



***
Qt SQL Programming  系列翻译
-  [SQL Programming （2）： Connection to Databases](https://conner.work/2018/07/23/SQL%20Programming%20%EF%BC%882%EF%BC%89%EF%BC%9A%20Connectiong%20to%20Databases/)
***