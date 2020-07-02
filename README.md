## 生成微博短链

### 1 功能

- 利用微博评论将长地址转为微博短链
- 将博文下的长地址评论删除

### 2 截图

![](https://pic.rmb.bdstatic.com/bjh/d74dc0848b57e6d76a1d4e2bd31621d6.png)

### 3 实现方法

1. 自己发一条微博，并设置为仅自己可见。或者首页上随便找一条微博都行

2. 找到该博文的mid。F12打开开发者工具→勾选左上角`Preserve log`→点击评论，就可以看到需要的mid

   ![](https://pic.rmb.bdstatic.com/bjh/a19f8abe155f415ce4ab48fb63ba7c9d.png)

   ![](https://pic.rmb.bdstatic.com/bjh/38ab774a331348608eaa8acc8d569c99.png)

3. 微博Cookie。还是在开发者工具中找到Cookie, 只需要**`SUB`**, 有效时间为1年。

### 4 源码

```python
import requests
import urllib.parse
import re

"""
功能：利用微博评论功能生成短链，并删除微博下的评论信息
"""
headers = {
      'Cookie': 'SUB=你的cookie',
      'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.116 Safari/537.36',
      'Referer': 'https://www.weibo.com',
      'Content-Type': 'application/x-www-form-urlencoded'
    }


def get_short_url(long_url):
    url = "https://www.weibo.com/aj/v6/comment/add"

    payload = urllib.parse.urlencode({
        'mid': '微博mid',
        'content': long_url
    })
    response = requests.post(url, headers=headers, data=payload)

    try:
        data = response.json()['data']['comment']
        short_url = re.search(r'(https?)://t.cn/\w+', data).group(0)
        comment_id = re.findall(r'comment_id="(.+\d)"', data)[-1]   # 评论id
        print('微博短链：' + short_url)
        # del_comment(comment_id)  # 需要删除评论，可以取消该行注释
    except:
        pass


# 删除评论
def del_comment(comment_id):
    url = 'https://www.weibo.com/aj/comment/del'

    payload = urllib.parse.urlencode({
        'mid': '微博mid',
        'cid': comment_id  # 评论id
    })
    response = requests.post(url, headers=headers, data=payload)
    try:
        if response.json()['code'] == '100000':
            print('评论已删除')
    except:
        pass


if __name__ == '__main__':
    get_short_url(input('请输入长地址：'))

```

### 5 快捷处理

将 .py 文件放在桌面(或者其他位置)，将如下代码保存为 `.bat` 文件。

```
@echo off
python weibo.py
pause
```

