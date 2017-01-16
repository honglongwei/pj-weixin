

### 发送提醒

```python
#!/bin/env python
#coding=utf-8

import urllib2
import urllib
import json
import sys

i = 0
a = ['人员1', '人员2', '人员3']

tmp_data={
          'corpid' : '',
          'corpsecret' : '',
      }

url="https://qyapi.weixin.qq.com/cgi-bin/gettoken?"

data = urllib.urlencode(tmp_data)
req = urllib2.Request(url, data)
response = urllib2.urlopen('https://qyapi.weixin.qq.com/cgi-bin/gettoken?corpid=&corpsecret=')
token=json.load(response)
url_send="https://qyapi.weixin.qq.com/cgi-bin/message/send?access_token=%s" % token['access_token']

send_data={
   "touser": "@all",
   "toparty": "@all",
   "totag": "test",
   "msgtype": "news",
   "agentid": '2',
   "news": {
       "articles":[
           {
               "title": "【周报系统】| 例会通知",
               "description": "Dear ALL,\n\n       本周部门例会将在17:00准时开始,请提前做好工作安排并及时提交周报,准时参加会议!\n\n     【本周例会主持人:  {0}】\n".format(a[i]),
           }]
   },
   "safe":0
}
req = urllib2.Request(url_send,json.dumps(send_data, ensure_ascii=False))
res = urllib2.urlopen(req)
ret = json.load(res)
print ret

```

### 人员轮替状态变更
```python
#! /usr/bin/env python
# -*- coding: utf-8 -*-

import commands

a = commands.getoutput("egrep 'i = ' /remind.py")
c = commands.getoutput("egrep 'i = ' /remind.py|awk -F ' ' '{print $3}'")
m = int(c) + int(1)
if m > 2:
    m = 0
else:
    m = int(c) + int(1)
cmd = "sed -i 's/\i = %s/\i = %s/g' /remind.py "%(c, m)
commands.getoutput(cmd)
```
