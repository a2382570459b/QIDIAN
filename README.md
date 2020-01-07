# 江湖外-数据纷争
| 项目|负责人 |完成度|
|---|---|---|
|数据收集|张杰|完成|
|数据清洗|张杰|完成|
|数据可视化|张杰|完成|
|网页页面美化（css、html）|陈荣、刘启伦、张杰|完成
|flask框架构建web应用|张杰、陈荣|完成

## 项目主题
主要从**纵横小说网**爬取不同年份的小说月票榜数据来分析这些年来小说江湖中的类别之争，哪些小说类别一直以来都是最受欢迎，哪些小说在近些年又犹异军突起，后来居上。希望这份可视化图能为那些想要进入小说界的人一个参考，写出受欢迎的小说。 

## 市场分析
目前网络上还未有相关为网络小说家提供参考的数据分析，我们的项目不仅能让外界人士对小说界的收益环境有清晰的了解，也能让无论是已处于小说界的，或是想要进入但还没进入该行业的人提供更加专业的价值参考，也能给那些想要从网络小说发展变化找取价值的人一个现有数据。
 
## 数据收集方法
- 开放数据集下载
- API读取
- 爬虫

本次项目数据因为数据量略大，外加上网上没有成型的数据集供下载，所以选用了爬虫方式收集。

下面是爬虫代码：

```python
import xlwt
from lxml import etree
import requests
import time


#伪装请求头
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36'
                  ' (KHTML, like Gecko) Chrome/70.0.3538.67 Safari/537.36'
    }

all_info_list = [] #存储每部小说的各种信息列表

#定义获取爬虫信息的函数
def get_info(url):
    res = requests.get(url, headers=headers)
    selector = etree.HTML(res.text)
  	#采用xpath方法对网页信息进行搜索
    infos = selector.xpath('//div[@class="rankpage_box"]/ul/li') #找到信息的循环点
    for info in infos:
        style = info.xpath('span[2]/a/text()')[0]
        title = info.xpath('span[3]/a/text()')[0]
        poll = info.xpath('span[6]/text()')[0]
        info_list = [style, title, poll]
        all_info_list.append(info_list)
    #爬取成功后等待两秒
    time.sleep(2)

#主程序入口
if __name__ == "__main__":
    urls = ['http://www.zongheng.com/rank/details.html?rt=1&d=0&r=2019012&c=0&p={}'.format(str(i)) for i in range(1, 3)]
    for url in urls:
        get_info(url)
    #写好excel表格中的各个属性名
    header = ['类别', '书名', '月票数']
    book = xlwt.Workbook(encoding='utf-8')
    sheet = book.add_sheet('sheet1')
    for h in range(len(header)):
        sheet.write(0, h, header[h])
    i = 1
   #将提取到的信息写入excel表格中
    for list in all_info_list:
        j = 0
        for data in list:
            sheet.write(i, j, data) 
            j += 1
        i += 1
    #将信息保存在D:/reptile目录下的xiaoshuo.xls文件中
    book.save('D:/图片/2019.12.xls')
    ```
    

## 数据清洗工具
- pandas

因为是自主爬取的代码，前期数据格式较杂所以数据清洗工作较为繁重。

## 数据可视化工具
- pyecharts
运用了不少全局变量以及系列变量。
## 页面美化工具
- html——排版
- css——轮播效果

## 最终成果
[pythonanywhere](http://chro.pythonanywhere.com/)




