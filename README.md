# 微信接口开发

### 获取token

```python

#!/usr/bin/env python  
# -*- coding: utf-8 -*-  

import urllib   
import urllib2
import json

def get_token():
    url = 'https://qyapi.weixin.qq.com/cgi-bin/gettoken?'   
    user_agent = 'Mozilla/4.0 (compatible; MSIE 5.5; Windows NT)'   
    values = {
              'corpid': 'xxxxxxxxxxxx',
              'corpsecret': 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
             }   
    headers = {'User-Agent': user_agent}   
    data = urllib.urlencode(values)   
    req = urllib2.Request(url, data, headers)   
    response = urllib2.urlopen(req)   
    res = response.read()   
    data = json.loads(res)   
    return data

```

### 给微信公众号发消息

```python

#!/usr/bin/env python
#_*_ coding:utf-8 _*_

import sys  
reload(sys)  
sys.setdefaultencoding('utf8') 


import urllib2
import urllib
import json
import httplib
import simplejson

def send_msg(token, content):
    try:
        data = {
           "touser": "@all",
           "toparty": "@all",
           "totag": "test",
           "msgtype": "text",
           "agentid": 1,
           "text": {
               "content": content,
           },
           "safe": 0
        }
        data = simplejson.dumps(data,ensure_ascii=False)
        req = urllib2.Request('https://qyapi.weixin.qq.com/cgi-bin/message/send?access_token={0}'.format(token))
        response = urllib2.urlopen(req, data)
        print response.read()
    except Exception,e:
        print str(e)

print send_msg(token, '测试内容')

```
