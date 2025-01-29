這集談 Static Routing  
需要手動設定


R1新增完介面的IP地址後  
各個介面會產生兩個route，一個是Local指向Router本身，一個是Conneted指向與之連接的網路

但是呢  
如果此時Router收到的目的地地址是在remote network  
那Router根本不知道怎麼傳遞這個packet

R2的remote network指的是 不與R2直接相連的network  
所以現在的狀態是R1會drop掉目的地為remote network的網路

現在來設定R3相關介面的IP地址  
一樣，最後記得no shutdown

再來設定R4相關介面的IP地址

那現在要怎麼設定才能讓PC1與PC4去互相溝通呢？

現在要我們要談一個概念Default Gateway

PC端可以直接與所連接的網路去溝通  
但是要把packet傳到LAN之外，必須先把packet傳給它們的default gateway  
default gateway is a old term for router, so it means default router  
所以就是想把packet傳到LAN之外，要先傳給預設的router

所以兩台PC都要去做設定  
先以PC1為例

iface eht0 inet static  
        address 192.168.1.10/24  
        gateway 192.168.1.1

去設定default gateway這件事，又可以被稱為default route  
It is a route to 0.0.0.0/0   
= all netmask bits set to 0. Includes all addresses from 0.0.0.0 to 255.255.255.255  
It's a route that include everything

The default route is the least specific route possible  
because it includes all IP addresses.  
0.0.0.0/0 = 4,294,967,296 = 2^32

A /32 route(ie Local route) is the most specific route possible  
because it specifies only one IP address  
192.168.1.1/32 = 1 IP Address

so the default route says
if there aren't any more specific matches  for this packets 
don't drop it
send it via this route instead


PC1想傳東西給PC4

Src. IP: 192.168.1.10  
Dst. IP: 192.168.4.10

那ethernet header的Src與Dst MAC Address會是甚麼呢？

on layer 3 PC1 wants to send packet to PC4  
So the destination is PC4's IP  
但呢 必須先把這個packet傳到default gateway，也就是R1


Dst. MAC: R1 G0/2 MAC// 那要怎麼知道這個MAC Address？  
Src. MAC PC1 eth0 MAC


那要怎麼知道這個R1 G0/2 MAC Address？  
PC1會送一個ARP request 給192.168.1.1(這個是R1 G0/2介面的IP)

(1)當R1收到從PC1來的frame  
會先de-encapsulate it(remove L2 header/trailer)  
然後再去查看packet的資料

(2)然後R1會去檢查routing table有沒有most-specific matching route

(3)現在R1 只有C與L類型的route(而我們是想傳資料從PC1→PC2 經過不同network)  
所以找不到matching route  
那此時router會對packet做甚麼事？這種狀況會drop掉packet

(4)想要正確的傳遞packet  
R1必須要有一個route 能夠傳遞到目的地網路 192.168.4.0/24  
routes必須告訴我們要傳到網路192.168.4.0/24的下一站是哪邊(next hop)？

(5)其實有兩種可能路徑：  
PC1→R1→R3→R4→PC4 // 這部影片我們會選這條路徑當示範  
PC1→R1→R2→R4→PC4

在真實環境中  
router是可以設定多條path到目的地的  
而且可以load balance  
可以選一條當主要的path，另一條當備用的path

在path中的每台router都需要兩個route  
一個route設定成192.168.1.0/24  
另一個route設定成192.168.4.0/24  
這樣的方式保證了two-way reachability(PC1可以傳遞packet給PC4、PC4可以傳遞packet給PC1)

*手動設定R1 routing table  
R1在設定好IP地址後，已經自動設定了Connected route與Local route  
那PC1要把資料傳到PC4的話，需要先經由R1→R3  
所以R1除了要能通到PC1所在的網路(connted route)  
也要能通往PC4，那，要通往PC4的下一站(next hop)就是R3  
R1與R3的相通這一個網路的R3的介面的IP為192.168.13.3

PC1 ←→直接相連 R1 ←→直接相連 R3(next-hop)→.........遙遠的網路、目標PC所在網路(192.168.4.0)  

所以我們要在R1設一個static route為192.168.13.3  
設定指令為  
R1(config)# ip route ip-address netmask net-hop  

R1(config)# ip route 192.168.4.0 255.255.255.0 192.168.13.3  
                     目標網路的IP   遮罩       next hop  

R1(config)# do show ip route  
查看routing table  
注意這一條S    192.168.4.0/24[1/0] via 192.168.13.3  
// [1/0]的意思事[administrative Distance/Metric]之後才會介紹

*手動設定R3 routing table


PC1→......R1←→R3  ←→  R4 → ......遙遠的目標網路，PC4所在的網路

所以R3要幫忙傳遞PC1與PC4的資料，要怎麼設定呢？兩個方向都要設定

(1)R3→PC1要通  
目標網路：PC1所在網路 192.168.4.0/24  
next hop：R1(192.168.13.1)

(2)R3→PC4要通  
目標網路：PC4所在網路 192.168.4.0/24  
next hop：R4(192.168.34.4)


R3(config)# ip route 192.168.4.0 255.255.255.0 192.168.13.1  
R3(config)# ip route 192.168.4.0 255.255.255.0 192.168.34.4

*手動設定R4 routing table  
設定方法與上述概念一樣，不多寫了

R1 R3 R4的IP Address與routing table設定好後  
那我們要來測試PC1 與 PC4之間到底有沒有通  
先到PC1來ping PC4

PC1:~$ ping 192.168.4.10

他說 如果能ping成功 其實代表的是PC1到PC4雙向都是可以通的！

除了可以用next-hop去設定下一站，還有另外一個方法叫做exit-interface

R2(config)# ip route ip-address netmask exit-interface    
意思是我們會從我們本身的哪一個介面把資料傳出去  
而不是直接指定下一站(next hop)的IP地址


假設R2想要傳資料到192.168.1.0/24 網路，也就是R2想要傳資料到PC1所在的網路  
next-hop是R1

那對於R2本身來說，資料從R2→R1，資料是從哪一個網路介面出去的呢？  
A：G0/0

所以指令這樣下  
R2(config)# ip route 192.168.1.0 255.255.255.0 g0/0

這個指令還有一個option可以用  
假設在R2，資料要傳到192.168.4.0網路  
next hop是R4  
我們可以這樣下指令// 這個指令也太囉嗦了吧？寫得好細  
R2(config)# ip route 192.168.4.0 255.255.255.0 g0/1 192.168.24.4
                     目標網路                由此出去  next-hop  


只有寫傳出去的介面，而沒寫next hop的話  
設定完，去查routing table，訊息就不會顯示next-hop的IP地址  
而且顯示直接連接目標網路，但，其實沒有阿，真正的直接連接目標網路的router是R1  
而不是我們這邊的R2  
S 192.168.1.0/24 is directly connected, GiabbitEthernet0/0  
這樣的方式會依賴於一個叫Proxy ARP的功能(Proxy ARP完全不在CCNA的範圍，所以不會教)


那有寫next-hop的話，就會顯示next-hop的IP地址  
S 192.168.4.0/24 [1/0] vis 192.168.24.4 GigabitEthernet0/1


最後 我們來設定router的default route  

* A default route is route to 0.0.0.0/0 // 最寬鬆的可能的route  

* 如果進來的packet找不到more specific route，就會用default route  

* 所以default route 通常被用來把traffic導引到Internet

* more specific route通常用在內部的網路(internal networks)  
東西被傳遞到internal networks之外，那那個之外就是Internet  
這時就會使用default route

此處的用法是default route常見的用途  
但deault route還有別的作用

查routing table  
如果顯示Gateway of last resort is not set  
意思是default route尚未被設定

那現在來設定default route囉  
R1(config)# ip route 0.0.0.0 0.0.0.0 203.0.133.2

設定完來查routing table

顯示S* 0.0.0.0/0 [1/0] 203.0.113.2

```
* - candidate default  
candidate可能有多個
```

Static Route configuraton  
R1(config)# ip route ip-address netmask next-hop  
R1(config)# ip route ip-address netmask exit-interface  
R1(config)# ip route ip-address netmask exit-interface next-hop