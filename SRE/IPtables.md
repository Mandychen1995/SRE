# IPTABLES 
**主要功能:
1.偽裝IP
2.封包傳遞的防火牆
3.產生虛擬IP**

------

封包進入: 發送人的IP+目的地IP
路由表: route -n
filter: 過濾資訊
forward: 開啟ipv4 ipforward功能

![p1](https://i.imgur.com/GJ4fOJN.jpg)

```
sudo apk add iptables
sudo iptables -L -n

名詞解釋:
t=table名 可以指定table default是filter

L=list

A=Append to chain

o=output

j=jump

!=否定的意思

d=destination

tcp=Transmission Control Protocol

dport=destination port

s=source
```

安裝iptables

![p2](https://i.imgur.com/gOtVxGZ.jpg)

阻擋網段

![p3](https://i.imgur.com/P479In2.jpg)

刪除阻擋的網段

![p4](https://i.imgur.com/Mw4HT0J.jpg)

![p5](https://i.imgur.com/zWrNyd6.jpg)






