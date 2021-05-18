# DHCP server
### 提供IP, netmask, network, broadcast, gateway, DNS 等IP

1. 先更改VMware Workstation 16 Player設定
選取host-only的change settings是因為要讓該網段下的client透過這層的server發放DHCP IP, 所以進去之後要取消use local DHCP這個選項,   
因為這個預設是要使用VMware中預設的DHCP, 取消之後才可以在指定的server中下載DHCP使用

![p1](https://i.imgur.com/ib1P8fp.png)

2. 建置DHCP server  
```
$ sudo apk update
$ sudo apk add dhcp
$ sudo nano /etc/dhcp/dhcpd.conf

subnet 10.3.32.0 netmask 255.255.224.0 {
  option domain-name "left.io";
  option domain-name-servers $R1IP2, 8.8.8.8;
  option routers $R1IP2;
  range 10.3.48.0 10.3.63.249;
}
```
#### 參數說明:  
subnet: 是要哪個網段之下的client收到DHCP排的IP  
netmask: 代表該選取網段的網路遮罩  
domain-name: 自己命名做區隔用途  
domain-name-servers: 是要當DHCP server的IP, 後面有8.8.8.8是當作備用IP  
range: DHCP IP的範圍
##### range計算說明:  
```
domain-name-servers 10.3.63.254
subnet 10.3.32.0
netmask 255.255.224.0

下一個網段是10.3.64.0, DHCP是後半段, 所以
(64-32)/2=16
32+16(後半段)=48
DHCP IP從10.3.48.0開始
參照行規扣除尾端廣播位置(255)&後5個IP(250-254)
DHCP IP結尾10.3.63.249
```

3. 編寫DHCP server設定檔  
把DHCP設定為當這台server開機就會自動執行
```
$ sudo rc-update add dhcpd
 * service dhcpd added to runlevel default
```


4. 啟動DHCP server  
```
$ sudo rc-service dhcpd start
 * Caching service dependencies ...                                       [ ok ]
 * /run/dhcp: correcting owner
 * /var/lib/dhcp/dhcpd.leases: creating file
 * /var/lib/dhcp/dhcpd.leases: correcting owner
 * Starting dhcpd ...                                                                  [ ok ]
```


