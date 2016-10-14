# 微信接口开发

```python


#!/usr/bin/env python  
# -*- coding: utf-8 -*-  

import urllib   
import urllib2
import json

def _tes():
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
