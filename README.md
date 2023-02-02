# CnkiSpider使用指南（by@zemengchuan）

## 用途：

CnkiSpider可以通过简单的代码实现高效的知网文章信息爬取，主要爬取的内容包括：【**标题、作者、发表时间、来源、链接**】，并将爬取的结果保存为CSV格式。经测试，某作者在知网上的821篇文章只需要2-4s即可全部获取（不同设备及网络情况可能会出现差异），效率相对而言比较高。

CnkiSpider的高效来自于采用了多线程的方式进行爬取。目前仅实现了**通过作者**的方式查询，将来还会持续更新通过其他的方式（如主题、篇关摘、关键词等）方式，还计划实现相关的图表分析功能，现在先将实现的部分上传供大家使用

## 优点

使用简单，效率较高

## 缺点

- 不够灵活，必须有作者的姓名、代码和第一机构才可以搜索，不能仅通过作者名字搜索（CnkiSpider设有专门的函数帮助确认作者的代码和第一机构，详情见使用方式）
- 目前仅支持中文搜索，英语搜索可能会出现问题
- 不支持自定义CSV保存路径
- 目前仅支持通过作者搜索

## 安装方式

```python
pip install CnkiSpider
```



## 使用方式

### AuthorSpider

Cnki目前的功能仅有通过作者搜索，所以只有AuthorSpider这一个类。使用方法如下：

- `AuthorSpider(author_name,author_code,institution)`只有三个参数，其中author_name是必填的。如果知道作者的代码和第一机构，那么后续的过程就会非常简单。

- 如果知道需要爬取的作者的姓名、代码和第一机构，那么可以按照如下操作获取结果：

```Python
from CnkiSpider import AuthorSpider

"""
将author_name,author_code,institution三个参数传入AuthorSpider中，
再使用get_all_article()方法即可快速获取该作者的所有文章
文件保存在当前目录下
"""

name = '钟南山' 
code = '000039361479' 
inst = '中国工程院'
cas = AuthorSpider(author_name=name,author_code=code,institution=inst)
cas.get_all_article()
"""
输出结果：

一共有文章820篇
共需要爬取17页
====================================================================================================
正在爬取第2页……
正在爬取第3页……
正在爬取第4页……
正在爬取第5页……
正在爬取第6页……
正在爬取第7页……
正在爬取第8页……
正在爬取第9页……
正在爬取第10页……
正在爬取第11页……
正在爬取第12页……
正在爬取第13页……
正在爬取第14页……
正在爬取第15页……
正在爬取第16页……
正在爬取第17页……
第17页爬取成功！第17页有20条数据
第2页爬取成功！第2页有50条数据
第5页爬取成功！第5页有50条数据
第10页爬取成功！第10页有50条数据
第13页爬取成功！第13页有50条数据
第14页爬取成功！第14页有50条数据
第7页爬取成功！第7页有50条数据
第16页爬取成功！第16页有50条数据
第9页爬取成功！第9页有50条数据
第4页爬取成功！第4页有50条数据
第6页爬取成功！第6页有50条数据
第12页爬取成功！第12页有50条数据
第11页爬取成功！第11页有50条数据
第8页爬取成功！第8页有50条数据
第3页爬取成功！第3页有50条数据
第15页爬取成功！第15页有50条数据
====================================================================================================
爬取完成，已将结果保存至./钟南山-中国工程院-000039361479/

"""
```

- 如果仅知道姓名，那么可以按照如下操作获取结果：

```python
from CnkiSpider import AuthorSpider

"""
如果只知道姓名，那么就需要author_recommend()函数的帮助
运行按照提示确定作者的代码和第一机构即可
最后使用get_all_recomment()方法获取所有文章，
如果get_all_recoment()获取的作者列表有误，可以输入re再次获取
文件保存在当前目录下
"""

cas = AuthorSpider('钟南山')
cas.author_recommend()
cas.get_all_article()

"""
author_recommend()会返回作者的姓名、代码和第一机构
如果有需要获取相关参数（姓名、代码、第一机构），可以按照如下的操作进行
"""
# cas = AuthorSpider('钟南山')
# print(cas.name,cas.code,cas.institution)
#author_name, author_code, institution = cas.author_recommend()
# print(cas.name,cas.code,cas.institution

"""
输出结果为：

    作者              机构
0  钟南山           中国工程院
1  钟南山
2  钟南山
3  钟南山      南昌大学第一附属医院
4  钟南山   共信医药科技股份有限公司;
5  钟南山          南风窗杂志社
6  钟南山         扎木县人民医院
7  钟南山
8  钟南山
9  钟南山  上海明品医学数据科技有限公司
请选择需要查询的作者序号(输入exit退出)：0
一共有文章820篇
共需要爬取17页
====================================================================================================
正在爬取第2页……
正在爬取第3页……
正在爬取第4页……
正在爬取第5页……正在爬取第6页……

正在爬取第7页……
正在爬取第8页……
正在爬取第9页……
正在爬取第10页……
正在爬取第11页……
正在爬取第12页……
正在爬取第13页……
正在爬取第14页……
正在爬取第15页……
正在爬取第16页……
正在爬取第17页……
第17页爬取成功！第17页有20条数据
第14页爬取成功！第14页有50条数据
第4页爬取成功！第4页有50条数据
第10页爬取成功！第10页有50条数据
第12页爬取成功！第12页有50条数据
第3页爬取成功！第3页有50条数据
第13页爬取成功！第13页有50条数据
第16页爬取成功！第16页有50条数据
第2页爬取成功！第2页有50条数据
第5页爬取成功！第5页有50条数据
第7页爬取成功！第7页有50条数据
第15页爬取成功！第15页有50条数据
第11页爬取成功！第11页有50条数据
第6页爬取成功！第6页有50条数据
第9页爬取成功！第9页有50条数据
第8页爬取成功！第8页有50条数据
====================================================================================================
爬取完成，已将结果保存至./钟南山-中国工程院-000039361479/
"""
```

## 计划

- 加入更多的搜索方式
  - 主题
  - 篇关摘
  - 关键词
  - 篇名
  - 全文
  - 第一作者
  - 通讯作者
  - 作者单位
  - 基金
  - 摘要
  - 小标题
  - 参考文献
  - 分类号
  - 文献来源
  - DOI
- 加入自定义保存路径
- 尝试用异步的方式，或许会有更高的效率
- ……
