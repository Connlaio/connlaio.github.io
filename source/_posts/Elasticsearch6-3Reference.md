---
title: Elasticsearch 6.3 参考文档（1）
date: 2018-07-24 21:12:28
tags: [Elasticsearch,翻译]
categories: 
- [编程]
- [Elasticsearch 6.3 系列翻译]
---



## 开始

  Elasticsearch 是一个开源的，高度可扩展性的全文检索和分析引擎。它允许你在近乎实时下存储，搜索，分析大量数据。它通常用作底层引擎（技术），为具有复杂搜索功能和要求的应用程序提供支持。

以下是Elasticsearch的一些简单用例：

- 你运行了一个允许用户检索所售商品的在线商店。在这个例子中，你可以使用Elasticsearch保存你的所有商品库存和目录，在用户搜索的时候提供建议。
- 你想要收集日志和交易数据，通过分析和挖掘这些数据来查看交易趋势，统计信息，总结和异常情况。在这个例子中，你可以使用 Logstash（Elasticsearch，Logstash，Kibana Stack的一部分功能）来收集，整合，分析这些数据，然后使用Logstash将结果反馈给Elasticsearch。一旦Elasticsearch获取了这些数据，你就可以通过查找，聚合来挖掘你所感兴趣的任何信息。
- 你运行了一个价格预警平台，它允许精打细算的客户制定一些规则。例如，我想买一个电子产品，在未来一个月内，如果有供应商的价格低于 xxx 的时候，我能够接到通知。在这个例子中，需要收集所有供应商的定价信息，然后推送到Elasticsearch中，通过Elasticsearch的反查找（过滤器）功能，将商品价格的变动和客户的需求进行匹配，一旦条件满足，则向客户发出通知。
- 假设你有数据分析或商业智能的需求，需要在大量数据的基础上（假设有百万条，甚至几十亿条数据）进行快速的调研，分析，可视化，定制化问卷。在这个例子里，你可以使用Elasticsearch存储数据，之后使用Kibana（Elasticsearch/Logstash/Kibana 的部分功能）来创建定制化的仪表盘，以实现重要数据的可视化。另外，基于这些数据，你可以使用Elasticsearch的聚合功能来执行复杂的商业信息查询工作。

接下来的教程，将引导你安装运行Elasticsearch，了解其中的机理，执行索引，查找，修改数据等一些基础操作。在教程结束的时候，你应该对于Elasticsearch有了一定的了解，知道Elasticsearch 是什么，它是怎么工作的，希望你能够受到一些启发，利用它创建复杂的搜索应用程序，或者进行数据挖掘的工作。
