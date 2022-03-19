

### Google 搜尋技巧


內容搜尋

- 交集搜尋： 關鍵字1+關鍵字2
  - 火鍋+吃不飽
- 排除搜尋： -排除關鍵字
  - 咖哩飯  做法  -youtube
- 精準搜尋： "關鍵字或字串"
  - "Linux  kernel  2.6.38.8"
- 不確定搜尋： [關鍵開頭]*[關鍵結尾]
  - windows  admin*r
- 其一搜尋： 關鍵字1  OR  關鍵字2
  - 350  Linux  OR  Shell


範圍搜尋 ( 應綜合使用，單獨使用不建議 )

- 時間之前搜尋： before:日期
  - DDR4  開箱  before:2018
- 時間之後搜尋： after:日期
  - 台中景點  after:2021/05
- 時間範圍搜尋： after:日期  before:日期
  - 肺炎  after:2019/07/01  before:2020/01/01
- 數字範圍搜尋： 數字..數字
  - 疫苗上市  2019..2020
  - 快樂水  100..200


主體搜尋 ( 應綜合使用，單獨使用不建議 )

- 網站搜尋： site:網址
  - 免費課程  site:https://www.tibame.com
- 標題搜尋： intitle:關鍵字
  - 蘋果手機  intitle:破解工具
- 內有網址搜尋： inurl:網址
  -  二手汽車   inurl:admin
- 標題搜尋： intext:關鍵字
  - 正版+免費  intext:序號


文件搜尋 ( 應綜合使用，單獨使用不建議 )

- 檔案搜尋： filetype:類型
  - Ubuntu+安裝步驟  filetype:pdf
  - 資安合約  filetype:docx


計算、轉換或換算 1/2

- 數學計算：
  - 689+689
  - 1450-609-817
  - 9.2%+4%
  - cos(3*x)+sin(1*x)
- 單位換算： 數量與單位  to  目標單位
  - 30公分  to  公尺
  - 45000台幣  to  美金
- 描述搜尋： 關鍵字[facts]
  - 人參[facts]

- 時間查詢：地區時間
  - 美國時間
  - 紐約時間
  - 日本時間
- 日出、日落查詢： 地區日出  [日期]
  - 阿里山日出 2099/01/01
  - 北海道日落
- 天氣查詢： 地區天氣
  - 阿里山天氣
  - 北海道天氣

Shodan ( 2009 ) ( https://www.shodan.io/  )
- 有暗黑 Google 之稱
  - 專門尋找與紀錄網路上的設備 ( Google 專門搜尋網站與網頁 )
  - 無時無刻搜尋與伺服器相關的設備
  - 各式伺服器、網路設備、無線設備、監視攝影機、印表機、門禁系統…
- 有自己的搜尋語法* ( 網站 )、CLI 指令、Chrome 擴充功能
  - net:168.95.1.1/24		country:tw
  - port:3389			title:監控
  - isp:hinet.net			version:1.3.1
  - hostname:www.tibame.com	product:zoom
- 為加入會員 ( 月費 ) 有搜尋次數限制、深度限制

-----

### 歷史頁面搜尋

- Google 快取儲存
  - 頁庫存檔，是 Google Web Cache 功能
    - 最新的歷史備份，作為快取或故障臨時取用

![image20220308191501.png](image20220308191501.png)


  - 快取頁面搜尋： cache:頁面網址
    - cache:www.dcard.tw

----


網站時光機 ( Wayback Machine, 1996, 2001 ) ( https://web.archive.org/ )
- 數位檔案館 ( INTERNET ARCHIVE, 2001 ) 的服務之一
  - 網頁*、圖書、影音、音樂、軟體、圖像…
  - 專門保留網路資訊的  爬蟲  網站
- 普遍取得所有知識 ( universal access to all knowledge )
- 儲存公開網站的副本
  - 多時間點，頻率與
- 網站失效、消失時的替代品
  - 顯示為主，內容無互動性，外部連結無法確保有效



## 主動蒐集


### 域名、主機查詢工具與使用

ping，本地端、遠端主機探測、查詢
- 確認主機存在
- 確認主機實體位址 ( 搭配 DNS 的被動蒐集 )
選項 ( 僅列較常使用 )
- 指令： ping  [選項]  目的地 IP/Host 位置
        -c  n   指定測試次數
        -i   n   封包發送間隔	-W  n   等待回覆封包時間
```
    root@Workstation: ~#  ping  168.95.1.1
    root@Workstation: ~#  ping  dns.hinet.net
    root@Workstation: ~#  ping  -c  1  168.95.1.1
    root@Workstation: ~#  ping  -c  1  -i  0.1  168.95.1.1
    root@Workstation: ~#  ping  -c  1  -W  10  168.95.1.1
```


fping，ping 的加強版
- 確認主機存在
- 確認網段那些主機存在
- 確認網段那些主機存在 ( 只試一次 ) 並統計與使用亂數資料
- 選項 ( 僅列較常使用 )

指令： fping  [選項]  目的地 IP/Host 位置
        -c  n   指定測試次數	-r          失敗重試 ( def 3 )		    -g         指定範圍
        -i   n   封包發送間隔	-t          等待回覆時間 ( def 500ms )
        -s        顯示總計結果	-R         使用亂數資料 ( ICMP )
```
    root@Workstation: ~#  fping  -c  2  168.95.1.1
    root@Workstation: ~#  fping  -g  192.168.0.0/24
    root@Workstation: ~#  fping  -r  1  -sRg  192.168.0.0/24
```


arping，本地端主機探測、查詢 1/2
- 確認區域網路中機器的 MAC Address
- 確認區域網路中是否有 IP 衝突
- 確認目標 (192.168.0.1) 的 Mac位址
- 確認10次目標 (192.168.0.1) 的 Mac 與 IP 是否為使用 ( 綁定狀態、使用狀態 )
- 代替目標方 (192.168.0.1) 更新指定方 (192.168.0.10) Mac位址 (確認目標方認識指定方)

選項 ( 僅列較常使用 )
- 指令： arping  [選項]  目的地 IP/Host/Mac 位置
        -c  n   指定測試次數
        -i        指定發送介面	-W  n   等待回覆封包時間
        -D      無止盡詢問，成功顯示 !
        -r        顯示回覆的 Mac	-R         顯示回覆的 IP
```
    root@Workstation: ~#  arping  -c  1  192.168.0.1
    root@Workstation: ~#  arping  -c  1  -i  ens38  192.168.3.50
    root@Workstation: ~#  arping  -c  5  -W  0.1  192.168.0.101
    root@Workstation: ~#  arping  -D  192.168.0.1  -rR
```


netdiscover，搜尋區域網路主機
- 確認所有網段的機器 ( 不建議 )
- 確認指定介面的網段所有機器 ( Finished! 需等待一會 )
- 被動蒐集機器 ( 時間 = 內容 )
選項 ( 僅列較常使用 )
- 指令： netdiscover  [選項]  [目的地網段]
        -i        指定發送介面	-r        範圍 ( 網段寫法 )
        -p       被動蒐集 ( 不主動發送要求 )
        -r        顯示回覆的 Mac	-R         顯示回覆的 IP
```
    root@Workstation: ~#  netdiscover
    root@Workstation: ~#  netdiscover  -i  ens33  -r  192.168.0.0/24
    root@Workstation: ~#  netdiscover  -p
```

nmap，主機服務掃描工具 1/3
- 確認網段那些主機存在
- 偵測 IP位址於 1-100 間那些有 80 Port 開啟
- 偵測 Gateway 主機所有資訊 ( 最完整，過程較久，易被發現 )
- 偵測 www.pchome.com.tw 主機有哪些 TCP 服務
- 偵測 168.95.1.1-168.95.1.254 主機有哪些 UDP 常見服務
- 偵測 ptt.cc 是哪種作業系統
- 偵測 ptt.cc 有哪些服務 ( 稍…有保護自己 )
- 偵測 ptt.cc 常見的服務是什麼版本

選項 ( 僅列較常使用 )
- 指令： nmap  [選項] 目的地 IP/Host 位置
	  -A      掃描 Port、Service、OS…	-sP    使用 ping 掃描主機是否存在
	  -p      掃描主機指定 Port
      -n      不解析 DNS 資訊 ( 反解 )
	  -sT    與目標完成三方交握		-sU    與目標傳遞 UDP
	  -oN   輸出文字檔			-oX    輸出XML檔
      -O     掃描主機 Port 與作業系統
      -sS    與目標不完成三方交握		-sV    偵測服務版本
	  
```
    root@Workstation: ~#  nmap  -sP  192.168.0.0/24
    root@Workstation: ~#  nmap  -p  80  192.168.0.1-100
    root@Workstation: ~#  nmap  -A  192.168.0.1
	root@Workstation: ~#  nmap  -sT  www.pchome.com.tw  -oN  pchomeT.txt
    root@Workstation: ~#  nmap  -n  -p  53,69,123,161  -sU  168.95.1.0/24
    root@Workstation: ~#  nmap  -O  ptt.cc
    root@Workstation: ~#  nmap  -sS  ptt.cc
    root@Workstation: ~#  nmap  -sV  -sT  -p  22,23,80,443  ptt.cc
```











