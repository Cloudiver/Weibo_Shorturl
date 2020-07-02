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

### 4 快捷处理

将 .py 文件放在桌面(或者其他位置)，将如下代码保存为 `.bat` 文件。

```
@echo off
python weibo.py
pause
```

