# Apline環境下的VM建置
至官網下載 [Apline](https://alpinelinux.org/downloads/)
**版本為X86_64位元版本**
![](https://i.imgur.com/xlDJ0B5.png)

--------
開啟VMware創建新VM
![](https://i.imgur.com/WuqzGAU.png)

目前最新版本號為3.13.4, 對應到Linux版本為5.10開頭
![](https://i.imgur.com/ys8pCyh.png)

![](https://i.imgur.com/Sw6voiQ.png)

**主機請用需求/功能命名**
儲存此台VM的資料夾可以另外設定一個自己比較容易找的到的路徑
![](https://i.imgur.com/yDSjVCi.png)

硬碟容量參考客戶需求, 基本上是500GB以上, **並把硬碟存成同一檔案**
通常不會選split, 因為fat的檔案儲存格式會用4GB為一單位把硬碟內容分割,到時候分割檔太多會很難管理
![](https://i.imgur.com/pdNMeRJ.png)

確認好後, 再去自訂化規格
![](https://i.imgur.com/AaMrhkU.png)

CPU幾核心要看這台vm要跑多少程式; Ram的配置也是一樣, 把用不到的配置刪除掉
![](https://i.imgur.com/Emvtus7.png)

**網路要選Nat服務, Bridge可能會裝不起來**
CD選用下載檔置入
![](https://i.imgur.com/qxIJtv1.png)

安裝完成後即可開機
![](https://i.imgur.com/kS1VQz9.png)

---------
login: **root此帳號為最高權限, 登入後才可做設定**
**-Setup-alpine**
![](https://i.imgur.com/isLicah.png)

**-鍵盤配置: tw/備用配置: tw
-Hostname設定只能用小寫英文/數字/-
-eth0為網卡; 若內建其他張網卡就會接續eth1,eth2…
-設定root密碼**
![](https://i.imgur.com/gjHa5Qh.png)

**-主時區: Asia/副時區: Taipei
-proxy代理伺服器: 將公司內部的IP偽裝成通成bridge的IP
若公司沒有提供就不用特別設定
-NTP時間協議: 時間要同步才能連的上網, 預設即可**
![](https://i.imgur.com/5Grv2Yh.png)

**-之後更新的載點位置, 通常選台灣地區的網址, 這樣會比較方便連線**
Ex. 安裝好後要Apk add 
![](https://i.imgur.com/jtgXYjS.png)

**-請選sda/sys: CD片安裝會放到硬碟中,使用sda硬碟給system使用**
**-請選y消除此虛擬機硬碟中的東西(通常來說新機裡面就是空內容)**
![](https://i.imgur.com/5OjYrdW.png)

**-安裝至硬碟後重新開機, 退出iso光碟片**
![](https://i.imgur.com/aVb7RhJ.png)

-------

# Apline安裝完後的指令設定
1. **apk update**
2. **下載常用的指令**
apk add tree nano sudo zip unzip curl wget grep bash procps
3. **nano /etc/sudoers**
在%wheel ALL 那邊 NOPASSWD:ALL
將前方#刪除後儲存
**4. adduser –s /bin/bash –h /home/username –D username**
addgroup username wheel
echo –e “username\nusername\n” | passwd username 
5. **sudo login username**
6. **因為root是通用帳號最高權限, 此帳號無法刪除只能停權**
sudo passwd –dl root
--------
查ip: ipaddr
重新設定密碼: sudo passwd username
刪除帳號: sudo deluser username
