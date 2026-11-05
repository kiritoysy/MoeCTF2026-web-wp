## 查分系统
直接看源码

## 归途
<img width="600" height="400" alt="image" src="https://github.com/user-attachments/assets/c52b997c-9435-4677-8723-1cbcdda6da39" />


key1：F12打开控制台会出现

key2：ctrl+u查看源码得到

key3：源码中会看到一个/getKey?id=3

<img width="600" alt="image" src="https://github.com/user-attachments/assets/4f232545-d5e9-4e5b-aa38-b08096ad0b60" />

key4：把id改成4，并且查看响应头也看到了key5，还提示发送POST请求可以获得key6

<img width="600" alt="image" src="https://github.com/user-attachments/assets/7619f6df-56a7-49e4-a416-53496e544cbc" />

<img width="600" height="333" alt="image" src="https://github.com/user-attachments/assets/c4ccc938-3186-467c-b14d-4060349abab8" />

<img width="600" height="221" alt="image" src="https://github.com/user-attachments/assets/9cea807c-7f3e-40b6-afe0-c8b0d6be3235" />

key7：访问login,先用游客登录再把cookie里面的user改成admin得到key7

<img width="600"  alt="image" src="https://github.com/user-attachments/assets/24afaf2e-c904-4bdd-ac5c-2dcd9f11f429" />

提示爬虫，访问robots.txt发现/f13ggggg目录
最后把所有key组合提交即可获得flag

<img width="600"  alt="image" src="https://github.com/user-attachments/assets/f5c1bfc3-a608-465d-9333-d73b50159a65" />

## 当零来敲门
SSH连接发现可以输入python命令，根据题目说明，要在短时间内发送1-500
写个脚本:
```python
import requests
import time

URL = "http://127.0.0.1:8080/chat"
s = requests.Session()
start = time.time()
for i in range(1, 501):
    try:
        r = s.post(
            URL,
            json={"user": "challenger", "msg": i},
            timeout=3
        )
        if i % 50 == 0:
            print(f"已发送 {i}/500，耗时 {time.time()-start:.2f} 秒")
    except Exception as e:
        print(f"发送 {i} 失败：{e}")
        break
print("总耗时：", round(time.time()-start, 2), "秒")
```

拿到flag:

<img width="600"  alt="image" src="https://github.com/user-attachments/assets/28dbcc1f-310b-413d-a214-5475c0bd0196" />


## 查分系统_revenge
打开源码看到信息都被加密了

<img width="1917" height="985" alt="image" src="https://github.com/user-attachments/assets/f2befaee-fd97-4dd7-b6f5-c7763894e01f" />

根据题目提示，需要key,应该就是学号
参考查分系统的信息，进行爆破即可获得flag

## MoeCatchTheFrogs
拿到9只青蛙即可获得flag,通过一直点击调查可以获得8只

<img width="1858" height="852" alt="image" src="https://github.com/user-attachments/assets/8df685cb-f335-44dc-bdb7-2d01cb581e40" />

F12可以看到有个session；
eyJmcm9ncyI6WzAsMSwyLDMsNCw1LDYsN119.auwsfQ.HR-_T_E24aJsqjcpmqgZwgFk8I4
三段式：内容+时间戳+签名
解码是：{
  "frogs": [
    0,
    1,
    2,
    3,
    4,
    5,
    6,
    7
  ]
}
要把8加上去，因为密钥未知，需要爆破，这里使用flask-unsign工具，用字典爆破


## 
