# SRE工程師系統基礎
### 1.認識程序與程式
program: 檔案需具有執行權 (chmod +x)
process: 泛指使用者在作業系統(Windows/MAC OS/Linux/AIX…)跑的軟體/程式(busybox網站…)

user把可以執行的program從硬碟載入至記憶體, 進入process之後會記錄啟用這個程式的人IP

![p1](https://i.imgur.com/22dy8xn.jpg)

-----

### 2.顯示程序資訊
( a ) 條列式顯示所有程序
`ps -eo user,pid,cmd,%mem,%cpu`

![p1](https://i.imgur.com/vn9Y7q1.png)

因為Alpine是一個比較精簡的軟體, 裡面的功能不見得全部都有, 所以要另外下載部分指令
 
![p2](https://i.imgur.com/IxBEhhv.png)

*busybox 多用在IOT裝置上因為程式檔很小, 可以執行linux很多指令。但小也有缺點, 因為它可以模擬的指令太多, 可能無法每個命令都全功能上線
![p3](https://i.imgur.com/ZL5OxcW.png)

![p4](https://i.imgur.com/H5sEe9f.png)

( b ) top批次顯示
如果今天電腦使用變慢, 下top可以看出當下佔使用量最多的process, 現行使用狀態
```
top  -bin  2  -d 1  >pid.txt
cat pid.txt
```
![p5](https://i.imgur.com/9Q5x2zL.png)

( c ) pstree
`pstree –ph(alpine-p就好/ubtune要-ph)`

有bash因為是透過powershell遠端連線, 若再開第二個powershell如下

**Alpine: Init: 開機後叫做第一個執行的程式
Ubuntu: Systemd: 開機後叫做第一個執行的程式**

![p6](https://i.imgur.com/5IClU8g.png)

![p7](https://i.imgur.com/gxsK17A.png)

### 3.子程序與父程序
```
echo $$ 查我自己程式的PID
$PPID 查父程式的PID
```

### 4.程序與環境變數

程式變數無法讓其他程式繼續繼承
若用export則其他程式就可以繼承此變數

### 5.程序與信號
### 6.背景執行程序

Ctrl + c 是中斷/ Ctrl + z 是暫停,背景還在執行
kill -2 PID/ kill -9 PID

### 7.程序優先順序

daemon: process在背景跑, 優先順序高 ex. Open ssh/ 資料庫/網站系統...
nice=友好程度越高優先順序越低
```
第一個參數代表運算的時間, 第二個參數代表名字
nano count1.sh 
#!/bin/bash

x="$1"
echo "$2" $(date)
while [ $x -gt 0 ]; do
    x=$(( x-1 ))
done
echo "$2" $(date) 

$ chmod +x count1.sh
```

![p8](https://i.imgur.com/V27G8lN.png)


-----------

## Process Security in Linux
### 1.prcess的防火牆系統: Seccomp
**Linux System Call & Seccomp (security computing mode)**
在執行階段的檔案linux有防護機制(防火牆) 
### 2.setuid
### 3.capabilities

-----------

## Linux 目錄與檔案之權限意義
### 1.權限對檔案的重要性
### 2.權限對目錄的重要性
### 3.使用者操作功能與權限