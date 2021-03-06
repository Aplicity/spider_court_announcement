# spider_court_announcement
爬取广西人民法院开庭公告

## 请求URL获取
某一法院的的法院公告地址：http://xnqfy.chinacourt.gov.cn/article/index/id/M0g3NzAwNjAwMiACAAA.shtml

里面有近年各个星期的法院开庭公告，以及一些其他公告，本次目标为先获取各个星期的法院开庭公告的URL，公告分6页分开展示，在公告地址后面加参数/page/x即可得到第x页第请求URL。
比如第2页公告的地址为：http://xnqfy.chinacourt.gov.cn/article/index/id/M0g3NzAwNjAwMiACAAA/page/2.shtml

![主页展示](https://github.com/Aplicity/spider_court_announcement/blob/master/iamges/%E4%B8%BB%E9%A1%B5%E6%98%BE%E7%A4%BA.png
)

对每一页的进行请求，通过正则获取所有各个星期的公告URL，正则如下：'/article/detail/[0-9]{4}/[0-9]{2}/id/[0-9]{7}.shtml'，获取到的URL有可能不是开庭公告，
对于获取到其他数据的，后面再处理。

## 解析网站
由于网站架构不太标准，建立两种解析架构，分别对应新旧不同类型的网站架构。一种是通过BeautifulSoup进行提取数据，一种是通过正则匹配相应的字段，
后面只保留['案号','案由','开庭时间']三个字段，并汇总到pandas.DataFrame中。
![公告显示](https://github.com/Aplicity/spider_court_announcement/blob/master/iamges/公告展示.png)

## 数据清洗
由于上面对同一个URL解析了两次，有可能出现数据重复的现象，需要对数据进行去重。另外，由于法院信息录入员录入数据的时候不规范，出现一些错漏，如开庭时间出现
'2014年004月'、'2013年00月'等，对此进行修正。另外，字段案由中的数据，为了方便分析，把“纠纷”两字去掉。为了方便后面重新读取数据分析，把数据保存到本地“data.csv”

## 数据分析
由于公告的时间不太连续，有些时间段的公告缺失，难以做到时间序列分析，就先做了一个案由类型的分组统计。
![案由类型](https://github.com/Aplicity/spider_court_announcement/blob/master/iamges/纠纷类型.png)

