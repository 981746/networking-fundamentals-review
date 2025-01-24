Both switches have an empty MAC address table, and all PCs have an empty ARP table.

1. If PC1 pings to PC3, what messages will be sent over the network, 
     and which devices will receive them?
A：
PC1要送資料到PC3，那必須要知道PC3的MAC Address
但是呢 PC1只知道PC3的IP
所以PC1 要發ARP reqeust 廣播給所有連接上的節點
那PC3會回一個ARP reply 告訴PC1它的MAC Address
這時，PC1的ARP Table就新增了PC3的MAC Address
然後ping服務就拿到目標PC3 MAC Address去傳遞資料
所以ping服務就開傳遞訊息使用的是ICMP Echo Request
而PC3收到ICMP Echo Request後，會回一個ICMP Echo Reply給PC1

如果PC1 去ping PC3
指令是ping 192.168.1.3
// 一個ping指令通常會送四次的ping
// 但是cisco設備預設是送5次的ping

(1)ARP Request(received by PC2 PC3 PC4)
(2)ARP Reply(received by PC1)
(3)ICMP Echo Request(received by PC3)
(4)ICMP Echo Reply(received by PC1)


2. Send the ping and use Packet Tracer's 'simulation mode' to verify your answer.

按simulation mode按鈕
按PC1→Desktop Tab→Command Prompt
// 那可以按terminal嗎 好奇欸

下指令：
ping 192.168.1.3


3. Use pings to generate network traffic and allow the switches to learn the MAC addresses 
     of all PCs on the network.

A:在ping的過程中，switch會自動學到新的MAC Address
所以我們就讓
PC1 ping PC3// SW1會學到PC1 and PC3 MAC Address
PC2 ping PC4// SW1會學到PC2 and PC4 MAC Address


現在離開simulation mode
老師回到ReamTime模式再去ping
因為simulation mode有時會有問題

我們在剛剛的Command Prompt 再ping 一次
ping 192.168.1.3


C:\>ping 192.168.1.3



接著再去PC2的Command Prompt畫面，去ping PC4
C:\>ping 192.168.1.4


4. Use 'show' commands on the switches to identify the MAC address of each PC.

現在可以點SW1 and SW2去查看MAC Address table
SW1→CLI→按一下enter→show mac address-table

SW1>en
SW1>show mac address-table
Vlan    Mac Address       Type        Ports
----    -----------       --------    -----

   1    0001.647b.3119    DYNAMIC     Gig0/1
   1    0004.9a6e.d870    DYNAMIC     Gig0/1
   1    0060.5c56.14d3    DYNAMIC     Fa0/2→PC1
   1    00d0.d3ad.9cab    DYNAMIC     Fa0/1→PC2

在SW1無法辨別Ports為Gig0/1的MAC是PC3還是PC4
所以現在到SW2查看mac address table

SW2#show mac address-table
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----

   1    0001.647b.3119    DYNAMIC     Fa0/2→PC4
   1    0004.9a6e.d870    DYNAMIC     Fa0/1→PC3
   1    0060.5c56.14d3    DYNAMIC     Gig0/1
   1    00d0.d3ad.9cab    DYNAMIC     Gig0/1





5. Clear the dynamic MAC addresses from the MAC address table of each switch.
直接在SW1、SW2的CLI下指令：
clear mac address-table dynamic

再查看是否清除成功show mac address-table
SW1#show mac address-table
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----

如果輸入clear mac address-table dynamic? // 多一個問號
畫面會回傳<cr>
表示沒有更多option可以用
所以在packet tracer這邊 只能一次把所有Dynamic MAC Address全部刪掉....
但是GNS3模擬器 可以針對Interface來刪 或是刪除單一的MAC Address等等





