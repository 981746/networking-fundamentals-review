決定IP packets到達目的地的路徑的程序 叫做routing  
router會把所有的已經知道目的地的routes紀錄在routing table  
當router收到packets，它會去看routing table 找出最佳路徑去傳遞packet  

主要有兩種routing methods  
1.Dynamic Routing  
使用dynamic routing protocols(像是OSPF)  
跟其它的設備嗎？自動去分享routing相關資訊  
並且建立routing tables

2.Static Routing  
網路工程師或管理員 手動在router裡去設定routes

一個route會告訴router  
(1)如果要把packet傳送到目的地X，那你應該要把packet傳到net-hop Y
next-hop = the next router in the path to the destination
// 好像就是把東西傳到下一站的意思，下一站是另外一個router

(2)如果目的地X與router是直接互相連接的狀況，那就把東西直接傳給目的地X  
(3)如果目的地X就是router本身的IP地址，router就自己把packet接收起來，不用再往下傳

例子  
有四台router互相連接  
他們之間代表了WAN(Wide Area Network) = a network theat extends over a large geographical area  
舉例來說 這四台router 可能各自在不同的城市 或不同的國家

然後R1連接一個LAN、R4連接一個LAN  
所以有兩個LAN

R1連接一個LAN的網路為192.168.1.0/24 →Class C IP地址  
R1與R2之間的網路為192.168.12.0/24  
R1與R3之間的網路為192.168.13.0/24  
R2與R4之間的網路為192.168.24.0/24  
R3與R4之間的網路為192.168.34.0/24  
R4連接一個LAN的網路為192.168.4.0/24

所以switch沒有IP地址喔？  
然後router的每個連接界面都有一個IP地址

本部影片主要談  
automatically to a router to its routing table  
not dynamic route or static route

先來設定IP地址  
設定R1的g0/0、g0/1、g0/2介面的IP地址  
只示範R1怎麼設定IP地址，其他就不示範了

查看R1的routing table  
R1# show ip route

ip tables包含兩個部分：上方的Codes與下方的routes

上方的Codes：list the different protocols which router to learn routes

用藍色特別highlight  
L - local  
a route to the actual IP address configured on the interce(with a/32 netmask)  
意思是，我們剛剛有幫R1設定IP 就是剛剛設定的IP  
到剛剛設定的IP的這個路徑  
其實就是R1到其自身的介面 這段路徑吧吧  
這段路徑就是在R1內部 所以算local 是這個意思嗎？

C - connected  
a route to the network the interface is connected to(with the actual netmask configured on the interface)  
connected好像代表的是與R1連接的網路IP之間的路徑


我們看ip tables的下半部  
有三個routes 前方寫著L


R1目前尚未做任何設定，就已經有六個routes  
所以還沒做任何設定，就已經有「自己內部網路介面的路徑」與「所連接網路的路徑」  
等兩個類型 6個路徑

當你下達no shutdown指令  
每個介面都會自動加入兩個路徑到routing table  
應該指的就是「自己內部網路介面的路徑」與「所連接網路的路徑」

local與connected路徑 不屬於影片一開始所說的dynamic routing與static routing類型

/32 網路遮罩 代表32bits都固定不能改變 那就代表整個IP本身
藉由這條路徑  
R1會知道 如果它收到一個packet 目的地是192.168.1.1/32  
那這個packet就是給R1本身

192.168.1.0/24

其實代表的範圍是192.168.1.0 ~ 192.168.1.255  
那如果R1收到一個packet，且packet的目的是在這個範圍  
R1就會把packet傳到G0/2

a route matches a packet's destination if the packet's destination IP addres is part of the network specified in the route

192.168.1.1/32 matches only 192.168.1.1  

現在談routes selection  
假設R1收到一個packet，目的IP是192.168.1.1

老師問can you see the problem here?  
欸  
192.168.1.0/24的範圍 0~255  
那R1的g0/2介面是192.168.1.1  
所以其實有兩條路可以都可以走吧？

那R1會選擇哪一條路徑呢？  
答案是it will choose the most specific matching route  
會選擇最明確 最精確的那條路  
所以會選R1的g0/2介面192.168.1.1  
所以R1會把這個packet給自己 而不是藉由g0/2把packet傳出去


most specific matching route = the matching route with the longest prefix length

下面這段文字是甚麼意思？  
192.168.1.0/24 is variable subnetted, 2 subtnets, 2 masks  
192.168.12.0/24 is variable subnetted, 2 subtnets, 2 masks  
192.168.13.0/24 is variable subnetted, 2 subtnets, 2 masks  

我們必須知道 這三行並不是routes  
我們先看第一行  
在routing table中  
在192.168.1.0/24的Class網路  
有兩個routes被subnets  
有兩個不同的遮罩

剩下的兩行意思雷同

// 目前課程尚未教到subnet  
// 這邊只是要說 這三行並不屬於routes

以上是Day 11 part1的內容