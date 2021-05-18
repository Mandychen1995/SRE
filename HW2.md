# 網路安全

### SSH免密碼連線 -非對稱式加密

![p1](https://i.imgur.com/X07C7V8.png)

-----
連線者: client  
被連線者: server  

![p2](https://i.imgur.com/sSk8VgH.png)

-----

![p3](https://i.imgur.com/tAizTW1.png)

-----
ssh 至叢集內所有 Linux 主機皆免密碼:

```
1. SSH時取消詢問
sudo nano /etc/ssh/ssh_config
StrictHosteyChecking no

2. 選一台主機當client, 產生公私鑰
$ ssh-keygen -t rsa -P ''
返回據有公鑰匙的目錄
$ cd .ssh

3. 把公鑰匙複製成authorized keys
$ cp id_rsa.pub authorized_keys
回到家目錄
cd ~ 

4. 用scp複製公鑰到各台主機, 這樣全部電腦相當於都使用同一扇大門, 用一把鑰匙就可以進入
$ scp -r ~/.ssh hostname@IP:~/

或是直接用hosts名連線
$ sudo nano /etc/hosts
(編輯IP-hostname)

$ scp -r .ssh r1:~/
$ scp -r .ssh r2:~/
$ scp -r .ssh r3:~/
$ scp -r .ssh r4:~/
```

