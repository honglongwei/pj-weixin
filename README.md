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
        data = simplejson.dumps(data, ensure_ascii=False)
        req = urllib2.Request('https://qyapi.weixin.qq.com/cgi-bin/message/send?access_token={0}'.format(token))
        response = urllib2.urlopen(req, data)
        print response.read()
    except Exception,e:
        print str(e)

print send_msg(token, '测试内容')

```

### zabbix微信告警

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import urllib,urllib2,json
import sys
reload(sys)
sys.setdefaultencoding( "utf-8" )


class WeChat(object):
    __token_id = ''
    # init attribute
    def __init__(self,url):
        self.__url = url.rstrip('/')
        self.__corpid = ''
        self.__secret = ''

    # Get TokenID
    def authID(self):
        params = {'corpid':self.__corpid, 'corpsecret':self.__secret}
        data = urllib.urlencode(params)
        content = self.getToken(data)
        try:
            self.__token_id = content['access_token']
            # print content['access_token']
        except KeyError:
            raise KeyError

    # Establish a connection
    def getToken(self,data,url_prefix='/'):
        url = self.__url + url_prefix + 'gettoken?'
        try:
            response = urllib2.Request(url + data)
        except KeyError:
            raise KeyError
        result = urllib2.urlopen(response)
        content = json.loads(result.read())
        return content

    # Get sendmessage url
    def postData(self,data,url_prefix='/'):
        url = self.__url + url_prefix + 'message/send?access_token=%s' % self.__token_id
        request = urllib2.Request(url,data)
        try:
            result = urllib2.urlopen(request)
        except urllib2.HTTPError as e:
            if hasattr(e,'reason'):
                print 'reason',e.reason
            elif hasattr(e,'code'):
                print 'code',e.code
            return 0
        else:
            content = json.loads(result.read())
            result.close()
        return content

    # send message
    def sendMessage(self,touser,message):
        self.authID()
        data = json.dumps({
                'touser':"@all",
                'toparty':"@all",
                'totag': "test",
                'msgtype':"text",
                'agentid':"2",
                'text':{
                        'content':message
                },
                'safe':"0"
        },ensure_ascii=False)
        response = self.postData(data)
        print response


if __name__ == '__main__':
        a = WeChat('https://qyapi.weixin.qq.com/cgi-bin')
        a.sendMessage(sys.argv[1],sys.argv[3])
```

