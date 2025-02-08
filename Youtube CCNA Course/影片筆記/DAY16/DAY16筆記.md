VLANs(Virtual Local Area Networks)  
Virtual LANs

What is a LAN?  
Broadcast Domains  
What is a VLAN?

what does a router do with a broadcast frame?  
會接收frame，但不會傳送到任何地方

這邊是R1與R2的point to point connection，這邊也算是一個broadcast domain

什麼是broadcast domain?  
一個broadcast domain是有一堆設備串在一起 形成一個group  
然後這個group裡的設備們會收到由group裡面的其中一個成員發送的  
broadcast frame(destination MAC FFFF.FFFF.FFFF)

這一個例子是  
有三個部分的PC都連到同一台switch  
然後這一台switch再跟一台router連  
但是呢不同部分是共享同一個網路 並沒有去切分子網路  
所以  
當工程部門的其中一台PC，傳一個broadcast frame到switch  
那所有部門的PC(發送方除外)都會收到broadcast frmae  
這樣其實不太好  
工程部門發broadcast frame，其他的部分也一起收到  
這樣有效能與安全的疑慮


效能方面  
沒有必要收到frmae的設備也一起收到  
這樣會影響網路的traffic

所以現在來為三個部門切子網路  
但是每個子網路與router，router必須要有每個子網路的IP地址  
因此這邊畫了三條線連接router與switch

假設現在工程部門的PC1要發送資料到銷售部門的PC2  
那PC1會先發送資料到R1  
Src IP 192.168.1.1// PC1 IP  
Dst IP 192.168.1.129// PC2 IP  
Src MAC: PC1 MAC  
Dst MAC: MAC R1 MAC

發送到R1後  
R1會把Src MAC與Dst MAC的內容改掉再發送給switch  
改成  
Src MAC: R1 MAC  
Dst MAC: PC2 MAC

switch收到後再發送給PC2  
所以現在PC1要發送到不同部門的PC，必須先經過R1這台router  
而router這邊就可以設定一些規則或防火牆去控管  
可是呢還是有問題  
如果要傳送的frame是unknown frame或unicast frame呢  
switch會flood frame out of all interfaces  
假設PC1發送了一個Dst MAC:FFFF.FFFF.FFFF的frame

switch will only aware of the layer 2  
所以縱使在IP層切了三個子網路，但是switch並不知道這件事  
PC1發送了一個Dst MAC:FFFF.FFFF.FFFF的frame  
然後switch會flood the frame  
所有與switch連接的設備又會全部收到frame


雖然我們在network(IP)層(layer3)切了三個子網路  
但是所有的設備仍然在同一個broadcast domain(layer2)

其中一個解法是  
為每個部門都安裝一台switch  
但是這種解法不太彈性，而且網路設備也不便宜

所以比較好的解法是使用VLANs去在layber2層面把不同部門的設備給切割開來

那VLAN要怎麼設定呢？在switch上設定  
更精確地說 是在switch的interfaces上設定


the switch does not perform inter-VLAN routing.  
It must send the traffic through the router  
哦哦 意思是PC1要發送資料到PC2(跨部門)  
switch並不能(在不同的VLAN間)直接幫忙傳  
而是要透過router  
PC1→switch→R1→switch→PC2

SW1#show vlan brief  
沒有做任何設定 所有的設備都會在VLAN1底下  
```
VLAN     Name           Status       Ports
1        default        active       所有與switch連接的設備都會列在這欄

1002
到
1005
1002到1005可以為old technology 可以忽略
```

所以VLAN1、1002~1005為預設值，而且不能刪除


那一個設備(interface)怎麼assign VLAN呢  
```
SW1(config)#interface range g1/0 - 3
SW1(config)#switchport mode access// set the interfaces as access port什麼意思？ 
// 所有與SW連接的設備 預設應該是access port 但是這邊還是手動去說明
SW1(config)#switchport acess vlan 10// 把vlan指派給ports
SW1(config)#do show vlan brief
```

設定好後 vlan 10 20 30的名稱會顯示
```
VLAN    Name
10      VLAN0010
20      VLAN0020
30      VLAN0030
```
那我們可以去把名稱改掉
```
SW1(config)#vlan 10// 這個指令同時也會新增一個vlan，但其實vlan 10已經新增過了
SW1(config-vlan)#name ENGINEERING
SW1(config-vlan)#vlan 20
SW1(config-vlan)#name HR
SW1(config-vlan)#vlan 30
SW1(config-vlan)#name SALES
```

An access port is a switchport which belongs to a single VLAN,  
and usually connects to end hosts like PCs

Switch ports which carry multiple VLANs are called 'trunk ports'.

如果在PC1 ping 255.255.255.255  
會發送broadcast frame  
那這個frame只會廣播給vlan10

問題一  
這張圖裡有幾個broadcast domain?  
答案是6  
// 我還想說 switch接一台PC的 算不算一個domain

問題二 答案是5 除了要計算VLAN，記得要算point to point connection

問題四  
PC3會傳給SW、router與另一台PC也會收到  
所以三個設備會收到  
// 我少算了switch