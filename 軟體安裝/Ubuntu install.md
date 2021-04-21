# Ubuntu環境下的VM建置
至官網下載 [Ubuntu](https://www.ubuntu-tw.org/modules/tinyd0/) 版本為20.04.2.0

![p1](https://i.imgur.com/xfId4eo.png)

![p2](https://i.imgur.com/B0nIFOO.png)

-----

開啟VMware創建新VM

![p3](https://i.imgur.com/xj4vMIm.png)

------

![p4](https://i.imgur.com/Uc5CFLH.png)

------

**命名方式請依照需求用途/軟體版本命名**
資料夾儲存處盡量設在C槽, 比較好找到
![p5](https://i.imgur.com/2ioFOw8.png)

---

硬碟設定現行一般都是500G以上, 可以再多預留空間給客戶

![p6](https://i.imgur.com/J7bwf5E.png)

---

RAM/CPU設定依照客戶需求判斷, 把用不到的功能remove, 尤其是USB, 避免資料外洩
CD/DVD選已經下載好的ISO

**不能在環境是host-only之下install, 先用nat或是bridge網域執行後再切回host-only**

![p7](https://i.imgur.com/fmGwrdb.png)

---

語系沒有中文, 請選擇英文

![p8](https://i.imgur.com/ewNAnEp.png)

---

美式鍵盤

![p9](https://i.imgur.com/ZD0qa8F.png)

---

設定IP, 預設都是DHCP
![p10](https://i.imgur.com/yz74bxi.png)

---

Proxy不用特別設定, 除非公司內部使用者只允許透過一個地址去連網

![p11](https://i.imgur.com/EpClKbt.png)

---

設定硬碟安裝, LVM為多個實體partition再一起, 日後若有預想使用空間不足即可點選此選項(已預設)

![p12](https://i.imgur.com/3d0ShKO.png)

---

![p13](https://i.imgur.com/2yYyA2x.png)

---

第一個user name通常會寫公司名稱;
第二個是主機名稱;
第三個user name才會是使用者名稱喔!

![p14](https://i.imgur.com/8dnL3I9.png)

---

**ssh請點選安裝, 這樣這台機器才可以遠端連線喔!**

![p15](https://i.imgur.com/04uwAtm.png)

---

此為snap安裝, 若有需要再自行設定

![p16](https://i.imgur.com/gX1S8VK.png)

---

等待安裝OpenSSH

![p17](https://i.imgur.com/P7xMxYP.png)

---

安裝完成後重新開機

![p18](https://i.imgur.com/1rnskdD.png)

---

重新開機後先退出ISO光碟片, 選回use physical drive
登入之後就可以下指令更新
```
Sudo apt-get update
Sudo apt-get upgrade
```

![p19](https://i.imgur.com/BV5dOF8.png)

![p20](https://i.imgur.com/xgXb5vs.png)
