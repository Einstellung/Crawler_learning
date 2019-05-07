# 示例：利用爬虫获取天气信息

> 利用爬虫获取天气信息，并通过邮件每天定时发送到特定邮箱
>
> 爬取的网站：中国天气网 <www.weather.com.cn/weather/101270101.shtml >

## 任务分析

我们可以将任务分为以下三个部分：

1. 爬取天气信息
2. 发送邮件
3. 定时进行任务

## 程序编写
### 爬取天气信息
打开[中国天气网成都市的天气预报](www.weather.com.cn/weather/101270101.shtml)，右键进入“检查”，进入“Network”，刷新查看第一个请求并“Preview”，发现显示出了我们所看到的网页。我们直接爬去该网页就可以了。
进入“Element”，选择左上角的“Select an element in the page to inpect it”按钮，找到关键元素。

![关键元素选取](http://a4.qpic.cn/psb?/V14faFn00phLM0/2nO29tEGCr*spruTUBj8iUaBkXznodEtLWn.IBySolc!/c/dL8AAAAAAAAA&ek=1&kp=1&pt=0&bo=FgYXBAAAAAARJxM!&tl=3&vuin=1581897229&tm=1557216000&sce=60-2-2&rf=0-0)

利用 BeautifulSoup 抓取日期、天气和温度三个信息，将函数封装，程序如下：

```python
import requests
from bs4 import BeautifulSoup

def get_weather():
    
    url = 'http://www.weather.com.cn/weather/101270101.shtml'
    headers = {
        'user-agent':'Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/74.0.3729.131 Safari/537.36'
    }
    
    res = requests.get(url,headers=headers)
    res.encoding='utf-8'
        
    soup = BeautifulSoup(res.text,'html.parser')
        
    date = soup.find(class_='t').find('h1')
    wea = soup.find(class_ ='wea')
    tem = soup.find(class_ = 'tem')
    return date,wea,tem
```

### 发送邮件

> 安全起见，程序我们需要自己输入发信人邮箱和密码，收件人邮箱和密码
>
> 程序使用qq邮箱，并开通邮箱中POP3/SMTP服务，一定记住授权码，并注意授权安全

```python
import smtplib
from email.mime.text import MIMEText
from email.header import Header

from_addr = input('请输入发信人邮箱：')
print('\n')
password = input('请输入密码')
print('\n')
to_addr = input('请输入收信人邮箱')
print('\n')

def send_email(date,wea,tem):
    global from_addr,password,to_addr
    smtp_server = 'smtp.qq.com'

    # 字符串strip方法：
    # Strip()方法用于删除开始或结尾的字符。
    # lstrip()|rstirp()分别从左右执行删除操作。
    # 默认情况下会删除空白或者换行符，也可以指定其他字符。
    text = '成都'+date.text+'\n天气：'+wea.text+'\n气温：'+tem.text.strip()

    msg = MIMEText(text,'plain','utf-8')
    msg['From'] = Header(from_addr)
    msg['TO'] = Header(to_addr)
    msg['Subject'] = Header('成都每日天气')

    server = smtplib.SMTP_SSL(smtp_server)
    server.connect(smtp_server,465)
    server.login(from_addr,password)

    try:
        server.sendmail(from_addr, to_addr, msg.as_string())
        print('邮件发送成功')
    except:
        print('邮件发送失败')

    server.quit()
```

### 自动发送邮件

我们使用 `schedule` 包来完成自动发送邮件的任务。

```python
import schedule
import time

def job():   
    date,wea,tem = get_weather()
    # 爬去天气信息
    send_email(date,wea,tem)
    # 发送邮件
    print('发送完成')

schedule.every().day.at("7:00").do(job) 
# 设定每天7:00发送邮件，可以修改时间间隔
while True:
    schedule.run_pending()
    time.sleep(1)
```

## 最终程序

> 程序需要在开机电脑上持续运行才能每天可以发送邮件，因此如果希望程序可以脱离电脑运行，还需搭建在云服务器上！

```python
import requests
from bs4 import BeautifulSoup

import smtplib
from email.mime.text import MIMEText
from email.header import Header

import schedule
import time

# ------------------------爬取天气信息----------------------------
def get_weather():
    
    url = 'http://www.weather.com.cn/weather/101270101.shtml'
    headers = {
        'user-agent':'Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/74.0.3729.131 Safari/537.36'
    }
    
    res = requests.get(url,headers=headers)
    res.encoding='utf-8'
        
    soup = BeautifulSoup(res.text,'html.parser')
        
    date = soup.find(class_='t').find('h1')
    wea = soup.find(class_ ='wea')
    tem = soup.find(class_ = 'tem')
    return date,wea,tem

# ------------------------发送邮件----------------------------
def send_email(date,wea,tem):
    global from_addr,password,to_addr
    smtp_server = 'smtp.qq.com'

    # 字符串strip方法：
    # Strip()方法用于删除开始或结尾的字符。
    # lstrip()|rstirp()分别从左右执行删除操作。
    # 默认情况下会删除空白或者换行符，也可以指定其他字符。
    text = '成都'+date.text+'\n天气：'+wea.text+'\n气温：'+tem.text.strip()

    msg = MIMEText(text,'plain','utf-8')
    msg['From'] = Header(from_addr)
    msg['TO'] = Header(to_addr)
    msg['Subject'] = Header('成都每日天气')

    server = smtplib.SMTP_SSL(smtp_server)
    server.connect(smtp_server,465)
    server.login(from_addr,password)

    try:
        server.sendmail(from_addr, to_addr, msg.as_string())
        print('邮件发送成功')
    except:
        print('邮件发送失败')

    server.quit()
    
from_addr = input('请输入发信人邮箱：')
print('\n')
password = input('请输入密码')
print('\n')
to_addr = input('请输入收信人邮箱')
print('\n')

# ------------------------定时----------------------------
def job():   
    date,wea,tem = get_weather()
    # 爬去天气信息
    send_email(date,wea,tem)
    # 发送邮件
    print('发送完成')

schedule.every().day.at("7:00").do(job) 
# 设定每天7:00发送邮件，可以修改时间间隔
while True:
    schedule.run_pending()
    time.sleep(1)
```

