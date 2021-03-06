# Crawl-Recruit-Data
抓取智联招聘和百度搜索的数据并进行分析,使用visual studio编写代码mongodb和SQLServer存储数据


使用scrapy框架结合 selenium爬取百度搜索数据，并进行简要的数据的分析！！

*爬取前的页面分析:

打开百度搜索页面，并查看网页源代码，问题便出现，无法查看到页面源代码，如下，只是返回一个状态说明，这时可以确定页面数据是动态生成，常规的爬取行不通。

![分类](https://github.com/Shadow-Hunter-X/Crawl-Recruit-Data/blob/master/BaiduDataSpider/res/1.png)

在浏览器中进行调试分析，可以发现需要定位使用的html元素，通过这一步至少可以将以下两个元素的XPATH或CSS Selector的表达式求解出来。 

![分类](https://github.com/Shadow-Hunter-X/Crawl-Recruit-Data/blob/master/BaiduDataSpider/res/2.png)

*制定爬取方案

既然搜索页面的内容是动态生成，常规的http请求后无法获取数据，针对这种问题的解决方法：

l  通过抓包工具，进行对http请求进行分析，找到实际数据请求的js代码后进行模拟请求获取数据，这种方法耗时耗力，且是无法适应页面更改的情况。

l  通过浏览器框架请求，并编写程序和浏览器通信获取数据分析，对于这种方法的选择有很多，如在windows上可以使用IE Browser控件，其他的可以使用其他内核的浏览器，这种方法的缺点是速度较慢。

l  这里选取的方法是使用 Selenium + Phantomjs的方法，这个结合scrapy也算是较为经典的一种方法。并且 Selenium + Phantomjs 也是作为Web应用程序进行自动化测试的一套方案。

l  Selenium : Selenium 是一个用于Web应用程序测试的工具，可以搭配主流浏览器进行使用，如 IE ，Chrome ，Firefox等

l  Phantomjs: 一个基于webkit内核的无头浏览器，即没有UI界面，即它就是一个浏览器，只是其内的点击、翻页等人为相关操作需要程序设计实现。

*编写爬虫代码

开始实际编写代码前，对爬取步骤的梳理。

         自动填写搜索关键字 – 自动触发搜索功能 – 抓取页面搜索数据（不包含广告推广项） – 分页跳转 ….. 

![分类](https://github.com/Shadow-Hunter-X/Crawl-Recruit-Data/blob/master/BaiduDataSpider/res/3.png)

 *关键代码:  
 实例化浏览器
 ![分类](https://github.com/Shadow-Hunter-X/Crawl-Recruit-Data/blob/master/BaiduDataSpider/res/4.png)
 
  l  输入关键字并进行查找，对关键字“IT教育”进行搜索
 ![分类](https://github.com/Shadow-Hunter-X/Crawl-Recruit-Data/blob/master/BaiduDataSpider/res/5.png)
   
  l  对第一页右边栏的“相关机构”（如下图）进行抓取（首先需要触发“展开”事件）
  ![分类](https://github.com/Shadow-Hunter-X/Crawl-Recruit-Data/blob/master/BaiduDataSpider/res/6.png)
  
  相关代码为：
  ![分类](https://github.com/Shadow-Hunter-X/Crawl-Recruit-Data/blob/master/BaiduDataSpider/res/7.png)
  
  在开启爬虫，进行爬取数据的，爬取结果如下：
  ![分类](https://github.com/Shadow-Hunter-X/Crawl-Recruit-Data/blob/master/BaiduDataSpider/spider-proc.png)
  
  
 *分析数据
 经过抓取，共抓取了76页，抓取的数据如下:
 Json文件
 ![分类](https://github.com/Shadow-Hunter-X/Crawl-Recruit-Data/blob/master/BaiduDataSpider/Spider1.png)
 在SQLServer数据库中
 ![分类](https://github.com/Shadow-Hunter-X/Crawl-Recruit-Data/blob/master/BaiduDataSpider/Spider-Res.JPG)
 
 对抓取数据进行关键字提取，并制作对应的标签云，得到的标签云图为.分析工具为python，通过jieba分词和pycloundtag两个模块进行，得到的分析结果如下：
 ![分类](https://github.com/Shadow-Hunter-X/Crawl-Recruit-Data/blob/master/BaiduDataSpider/Spider-res2.JPG)
  
分析搜索“IT教育”得到结果得出的初步结论，出现次数较多：

n  城市： 北京 深圳 杭州 武汉 长沙 等

n  机构： 北大青鸟 达内 传智播客 等

n  语言： java php html5 等
