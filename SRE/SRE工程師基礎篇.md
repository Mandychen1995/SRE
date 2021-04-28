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
在linux執行中程式的防火牆

### 2.setuid/setgid/sticky

**setuid=4, 
setgid=2, 
sticky=1**

#### ( A ) setuid
當一個可執行文件啟動時, 不會以啟動它的用戶的權限運行, 而是以該文件所有者的權限運行 >>>掠奪使用者權限

ex. desktop建立一個檔案, 但若把某指令的執行權限setuid, 之後用該指令權限執行的程式owner會更改。
![uid](https://i.imgur.com/1RqpUyh.jpg)


#### ( B ) setgid
對文件和目錄都有影響, 只要其他使用者備加入主要owner的群組, 他就可以在群組內建立/刪除檔案與目錄

```
esktop@UB2004D:~$ sudo useradd -m -s /bin/sh joe

desktop@UB2004D:~$ sudo passwd joe

desktop@UB2004D:~$ ls -al | grep test

drwxrwxr-x  2 desktop desktop  4096  四  28 18:13 test

desktop@UB2004D:~$ sudo login joe

$ cd /home/desktop/test

$ nano 1
```
就會發現無法在這個目錄中新增內容
![](https://i.imgur.com/PyenX1b.png)

但desktop若更新這一個特定目錄/檔案的執行權限setgid, 並把joe加入至desktop的群組中, 他就可以在這一個目錄中新增/刪除任何資訊

```
esktop@UB2004D:~$ chmod 2775 test

desktop@UB2004D:~$ ls -al | grep test

drwxrwsr-x  2 desktop desktop  4096  四  28 18:13 test

esktop@UB2004D:~$ sudo addgroup joe desktop
Adding user joe to group desktop ...

Adding user joe to group desktop

Done.
```


#### ( C ) sticky
只有owner可以刪除/移動這個指定目錄下的檔案, 其他使用者只有修改內容的權限

```
desktop@UB2004D:~$ chmod 1777 test

desktop@UB2004D:~$ ls -al | grep test

drwxrwsrwt  2 desktop desktop  4096  四  28 18:41 test

desktop@UB2004D:~$ cd test

desktop@UB2004D:~/test$ touch 123

desktop@UB2004D:~/test$ sudo login joe

$ cd /home/desktop/test

$ ls -al

-rw-rw-r--  1 root    desktop    0  四  28 18:40 123

$ rm 123

rm: cannot remove '123': Operation not permitted
```


### 3.capabilities

列出有設定 Linux capabilities 的所有命令
`getcap -r / 2>/dev/null`

從根目錄開始搜尋只要沒有capabilities的目錄就導入到黑洞, 剩下的就是具有cap特別功能的目錄


-----------

## Linux 目錄與檔案之權限意義

### 1.權限對檔案的重要性
### 2.權限對目錄的重要性
### 3.使用者操作功能與權限
R: cat/tree/ls/head/tail/cp/mv

W: nano/touch/cp/mkdir/rm;rm -r
#### rm: 被刪除的檔案/目錄要有W權限

X: cd目錄/執行檔案