## 爬取Elasticsearch 英文文档内容

直接上代码

```
# -*- coding: utf-8 -*-

import requests
from lxml import etree
from bs4 import BeautifulSoup

tocUrl = "https://www.elastic.co/guide/en/elasticsearch/reference/current/toc.html"

r = requests.get(tocUrl)

soup = BeautifulSoup(r.content, 'html.parser')


basePageUrl = "https://www.elastic.co/guide/en/elasticsearch/reference/current/"

# 获取所有的连接页面
for k in soup.find_all('a'):
    fullUrl = basePageUrl + k['href']
    print(fullUrl)
    resp = requests.get(fullUrl)
    con = etree.HTML(resp.content)
    part = con.xpath('//div[@class="part"]/div/*/text() | //div[@class="section"]/*/text() | //div[@class="chapter"]/*/text() | //div[@class="xpack section"]/*/text() ')
    f = open(k['href'], 'w+', encoding="utf-8")
    for t in part:
        # print(t.replace("\r\n", " "))
        f.write(t.replace("\r\n", " "))
    f.close()


```