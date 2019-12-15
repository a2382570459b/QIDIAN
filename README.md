# 博物馆“朗读者”app
| Target release | 12-4-2019 |
| ------ | ------ |
| Document Status | 未完成 |
| Document owner | 张杰 |
| Designer | 张杰 |
| Developer | 张杰 |
| Tester | 张杰 |
| API | 百度在线语音合成API 、百度相同图片搜索API| 


## 一、产品文档价值主张
### 产品介绍（加值宣言、使用场景）
近年来，博物馆游览人数在以每年一亿的数量增长，国家宝藏、如果国宝会说话等综艺节目更是成为爆款，从这些数据不难看出人们对精神文化的追求更高了。可观赏主体同样是具有文明意义的文物，为什么《如果国宝会说话》在豆瓣评分可高达9.4，博物馆的声评却有好有坏，是因为国宝会说话吗。还真是，
我们到博物馆是是如何欣赏文物的，靠自身的理解，靠展品生硬的文字描述，或是靠身边的人讲解。然而阅览欣赏需要一定的注意力，身边若有吵闹声或者其他的讲解声又会让这个体验大大降低。怎么解决，让博物馆会说话，**结合人工智能的语音合成技术**，利用百度在线语音合成api打造出一个具有朗读叙说功能的app供各个博物馆使用。**百度语音api**是将文本转换为音频朗读出来，它可提供多场景音库以及多种参数配置让用户满意的声音，同时文本可以由专业人士打造出流畅的故事。如果你不熟悉博物馆，在进入博物馆后可以选择戴上耳机并打开app，在途经展品时遇到感兴趣的文物可以进行“扫一扫”，扫得的图片会上传并且在**相同图片搜索api**的功能下与自建图库内图片进行匹配，然后返回相应的数据与启动朗读功能，app会自动以流畅的声音给你讲述展品中文物的声音，如果你走累了你也可以坐在小板凳上，自行搜索文物，去聆听了解每件展览品其背后的文化意义。

### 核心价值
抛去其他功能，这款app最核心的就是，能够以流畅的语音给用户讲述博物馆内展品具有文化价值意义的故事，声音能够提高人的亲切感，增加人的信任感，同时摆脱了单调
的文字叙述，博物馆也能因此少了哄吵。
搭配搜索匹配文物的功能可谓锦上添花相辅相成，方便用户使用价值。

### 用户痛点细化
将用户痛点结合具体使用场景进行表述（在产品介绍中已阐释）
- **痛点一**：想认真阅览不了解的文物的介绍时，却因博物馆周边吵杂，导致读不进去，没办法静静欣赏。
*产品加值*：
- **痛点二**：想了解文物故事，却疲于阅读文字。又苦于无人讲解。
- **痛点三**：博物馆地大物博，还诶探索完几件物品就走累了。

### 人工智能概率性与用户痛点
上述的用户痛点，我提出都可以结合人工智能的api来解决，这里我列出了百度大脑的智能语音合成api和百度的相同图片搜索api。
但这当中人工智能api是否可能会出现失败率？
我们都知道人工智能出现误判可能性是很大的，容易出现文不对本的情况，但在我们的产品中，api是与数据库一一对应的，出错可能性十分之低。如下：
- **百度语音合成api**：我已测试该api的转译准确度，无论我提供怎样混乱的文本，它都能准确的朗读出来并且无误，还能提供不同的语速语调声调，而我们的产品只需要该api将我们事先准备好的文本进行转译就行了，所以准确度可高达百分之九十九以上。
- **相同图片搜索api**：百度相同图片搜索api的数据源是自建库，且支持找到局部内容相同的大图，或适度调整背景和角度的相同图片。该api存在可能误判的因素，如果用户拍摄文物的角度叼转或者主体不明，可能出现判断错误，当然这也是可以通过设置提示来让用户拍准照片，然后照片会和自建图库匹配，已测试准确率百分之百。还有需要检索图和入库的原图要保持场景一致性，否则也有可能出现误差。
### 需求列表与人工智能API加值
| 需求| API|
| ------ | ------ |
|了解文物信息 | 设置好的文本以及语音合成技术，还有相同图片搜索技术方便用户查询文物信息|
|清晰展品位置|届时有自行设计的博物馆展品地图
|环境安静优美|语音合成技术能让彼此之间减少交流谈话，将关注点集中在文物以及朗读文物故事的声音。|
|公共设施完善|待|
|有良好的供应链|待|
## 二、原型设计
### 交互及界面设计
### 信息设计
### 原型文档
### 口头操作说明
## 三、API产品使用关键AI或机器学习之API的输出入展示
### API使用
#### 百度语音合成api
```
import urllib
from urllib.parse import urlencode
from urllib import request
import requests
from urllib.request import urlopen
import json

host = 'https://openapi.baidu.com/oauth/2.0/token?grant_type=client_credentials&client_id=ZNQXl8BbdL8c2PODqKgAqoqc&client_secret=SSYPWym3vIBnjG1z9Ycvl4GenamyUsoq'
headers = {
    'Content-Type': 'application/x-www-form-urlencoded'
}
res = requests.get(url=host, headers=headers).json()
access_token=res['access_token']

request_url = "http://tsn.baidu.com/text2audio"
params = {"tex":"床前明月光，疑是地上霜",
          "tok":access_token,
          "cuid":"00-FF-D6-C0-9E-E9",
          'ctp':1,
          'lan':"zh",     # 中文
          'spd':5,        # 语速
          'pit':5,        # 语调
          'vol':5,        # 音量
          'per':106,      # 男女声，106是度博文
          'aue':3}        # 音频格式，3是mp3
try:
    r = requests.post(request_url, params = params)
    print(r.headers)   # 返回的表头
    text = r.content   # mp3二进制数据
    
    # 将mp3的二进制数据保存到本地的mp3
    f = open("语音合成.mp3", "wb")
    f.write(text)
    f.close()
except Exception as e:
    print(e)

```

#### 百度相同图片搜索api
##### 上传
```
import base64
import urllib
from typing import BinaryIO
from urllib.parse import urlencode
from urllib import request
import requests
from urllib.request import urlopen
import json

host = 'https://aip.baidubce.com/oauth/2.0/token?grant_type=client_credentials&client_id=7k6PKcANGBCUCdHswpi9FD2N&client_secret=whefVZOK6OSPChoBTg98COom8wMmnqzv'
headers = {
    'Content-Type': 'application/x-www-form-urlencoded'
}
res = requests.get(url=host, headers=headers).json()
access_token=res['access_token']

request_url = "https://aip.baidubce.com/rest/2.0/realtime_search/same_hq/add"

f = open('h1.jpg', 'rb')
img = base64.b64encode(f.read())
params = {"brief":"{\"name\":\"小度\", \"id\":\"1\"}","image":img,"tags":"1,1"}
request_url = request_url + "?access_token=" + access_token
response = requests.post(request_url, data=params, headers=headers)
if response:
    print (response.json())
```
##### 检索
```
request_url = "https://aip.baidubce.com/rest/2.0/realtime_search/same_hq/search"
f = open('h2.jpg', 'rb')
img = base64.b64encode(f.read())

params = {"image":img}
request_url = request_url + "?access_token=" + access_token
headers = {'content-type': 'application/x-www-form-urlencoded'}
response = requests.post(request_url, data=params, headers=headers)
if response:
    print (response.json())
```
##### 删除
```
request_url = "https://aip.baidubce.com/rest/2.0/realtime_search/same_hq/delete"

cont_sign = '979961658,2726020645'
params = {"image":img}
request_url = request_url + "?access_token=" + access_token
headers = {'content-type': 'application/x-www-form-urlencoded'}
response = requests.post(request_url, data=params, headers=headers)
if response:
    print (response.json())
```
##### 更新
```
request_url = "https://aip.baidubce.com/rest/2.0/realtime_search/same_hq/update"
# 二进制方式打开图片文件
f = open('h4.jpg', 'rb')
img = base64.b64encode(f.read())

params = {"image":img}
request_url = request_url + "?access_token=" + access_token
headers = {'content-type': 'application/x-www-form-urlencoded'}
response = requests.post(request_url, data=params, headers=headers)
if response:
    print (response.json())
```
### 比较分析
### 风险报告
