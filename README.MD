





題目一：蒐集網域 president.gov.tw 一切資訊

目標：
(1) 使用各種手段收集一切資訊
(2) 如果可以，盡量別讓人知道是你...

驗證：
(1) 說明各項資訊如何查詢，被動收集？主動蒐集？其他？
(2) 確認資訊與 president.gov.tw 有關
(3) 各項資訊的說明、實際使用、或推測(或無使用)

內容：闡述方式、此方式優缺點、實行步驟...

-----



### 被動收集


nslookup
```
┌──(kali㉿kali)-[~]
└─$ nslookup www.president.gov.tw
Server:         192.168.211.2
Address:        192.168.211.2#53

Non-authoritative answer:
Name:   www.president.gov.tw
Address: 117.56.6.1
Name:   www.president.gov.tw
Address: 2001:4420:67f0:134::1
```
>
IPv4=117.56.6.1
IPv6=2001:4420:67f0:134::1


host
```
┌──(kali㉿kali)-[~]
└─$ host -v www.president.gov.tw
Trying "www.president.gov.tw"
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 62174
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;www.president.gov.tw.          IN      A

;; ANSWER SECTION:
www.president.gov.tw.   5       IN      A       117.56.6.1

Received 54 bytes from 192.168.211.2#53 in 8 ms
Trying "www.president.gov.tw"
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 5969
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;www.president.gov.tw.          IN      AAAA

;; ANSWER SECTION:
www.president.gov.tw.   5       IN      AAAA    2001:4420:67f0:134::1

Received 66 bytes from 192.168.211.2#53 in 4 ms
Trying "www.president.gov.tw"
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 50843
;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 0

;; QUESTION SECTION:
;www.president.gov.tw.          IN      MX

;; AUTHORITY SECTION:
president.gov.tw.       5       IN      SOA     admns1.gsn.gov.tw. hostmaster.gsn.gov.tw. 1646506801 3600 1800 1209600 3600

Received 96 bytes from 192.168.211.2#53 in 12 ms
```

## 主動收集

### TCP服務偵測 : 那些 TCP 常見服務於 117.56.6.1 開啟

scapy
```
>>> IP( ).display( )
###[ IP ]### 
  version= 4
  ihl= None
  tos= 0x0
  len= None
  id= 1
  flags= 
  frag= 0
  ttl= 64
  proto= hopopt
  chksum= None
  src= 127.0.0.1
  dst= 127.0.0.1
  \options\

>>> TCP( ).display( )
###[ TCP ]### 
  sport= ftp_data
  dport= http
  seq= 0
  ack= 0
  dataofs= None
  reserved= 0
  flags= S
  window= 8192
  chksum= None
  urgptr= 0
  options= []

>>> target="117.56.6.1"
>>> tport=(1,9999)
>>> askTcp=IP(dst=target)/TCP(sport=2468,dport=tport,flags="S")
>>> ans,unans=sr(askTcp,timeout=5,inter=0,verbose=0)
>>> ans.display( )
0000 IP / TCP 192.168.211.137:2468 > 117.56.6.1:ping S ==> IP / TCP 117.56.6.1:smtp > 192.168.211.137:2468 SA / Padding
0001 IP / TCP 192.168.211.137:2468 > 117.56.6.1:http S ==> IP / TCP 117.56.6.1:http > 192.168.211.137:2468 SA / Padding
0002 IP / TCP 192.168.211.137:2468 > 117.56.6.1:https S ==> IP / TCP 117.56.6.1:https > 192.168.211.137:2468 SA / Padding

```
>
找到已開啟tcp服務
117.56.6.1:smtp
117.56.6.1:http
117.56.6.1:https

### UDP服務偵測 : 那些 UDP 常見服務於 117.56.6.1 開啟
scapy
```
>>> IP( ).display( )
###[ IP ]### 
  version= 4
  ihl= None
  tos= 0x0
  len= None
  id= 1
  flags= 
  frag= 0
  ttl= 64
  proto= hopopt
  chksum= None
  src= 127.0.0.1
  dst= 127.0.0.1
  \options\

>>> UDP( ).display( )
###[ UDP ]### 
  sport= domain
  dport= domain
  len= None
  chksum= None

>>> target="117.56.6.1"
>>> tport=(1,9999)
>>> askUdp=IP(dst=target)/UDP(sport=RandShort( ),dport=tport)/"HELLO"
>>> ans,unans=sr(askUdp,timeout=5,inter=0,verbose=0)
>>> ans.display( )
>>> 
```
>
沒找到已開啟udp的服務

### 使用 macchanger 更改 MAC address
```
## 查詢我們當前的 MAC address
macchanger -s eth0

## 使用`ifconfig`命令關閉您的網絡接口：
ifconfig eth0 down

## 將網卡的硬件 MAC 地址更改為一些隨機的十六進制數了：
macchanger -r eth0

## 啟動您的網絡接口並顯示您的新 MAC 地址：
ifconfig eth0 down
macchanger -s eth0

```















