1. Configure the switch interfaces connected to PCs as access ports in the correct VLAN.  

2. Configure the connection between SW1 and SW2 as a trunk, allowing only the necessary VLANs.  
    Configure an unused VLAN as the native VLAN.  
    Make sure all necessary VLANs exist on each switch

3. Configure the connection between SW2 and R1 using 'router on a stick'.  
     Assign the last usable address of each subnet to R1's subinterfaces.

4. Test connectivity by pinging between PCs.  All PCs should be able to reach each other.


// 作答第一題  
1. Configure the switch interfaces connected to PCs as access ports in the correct VLAN.  
第一題要我們用access port來區分VLAN

VLAN10  
SW1 f0/1、f0/2這兩個要設為access port，並指派給VLAN10  
SW2 f0/2、f0/3這兩個要設為access port，並指派給VLAN10


SW1(config)#interface range f0/1 - 2  
SW1(config)#switchport mode access  
SW1(config)#switchport access vlan 10  
SW1(config)#do show vlan brief

SW2(config)#interface range f0/2 - 3  
SW2(config-if-range)#switchport mode access  
SW2(config-if-range)#switchport access vlan 10  
SW2(config-if-range)#do sh vlan br





VLAN20  
SW2 f0/1這一個要設為access port，並指派給VLAN20

SW2(config-if-range)#int f0/1  
SW2(config-if)#switchport mode access  
SW2(config-if)#switchport access vlan 20  
% Access VLAN does not exist. Creating vlan 20  
SW2(config-if)#do sh vl br


VLAN30  
SW1 f0/3、f0/4這兩個要設為access port，並指派給VLAN30

SW1(config-if-range)#int range f0/3 - 4  
SW1(config-if-range)#switchport mode access  
SW1(config-if-range)#switchport access vlan 30  
% Access VLAN does not exist. Creating vlan 30  
SW1(config-if-range)#do sh vl br


// 作答第二題  
2. Configure the connection between SW1 and SW2 as a trunk, allowing only the necessary VLANs.  
    Configure an unused VLAN as the native VLAN.  
    Make sure all necessary VLANs exist on each switch


SW1 == SW2  
VLAN10 與 VLAN30需要通過SW1 and SW2之間  
要用trunk port的方式同時讓這兩個VLAN通過


SW1(config)#int g0/1  
SW1(config)#switchport mode trunk  
SW1(config)#switchport trunk allowed vlan10,30  
SW1(config)#switchport trunk native vlan 1001  
SW1(config)#do sh int trunk

SW2(config)#int g0/1  
SW2(config)#switchport mode trunk  
SW2(config)#switchport trunk allowed vlan10,30  
SW2(config)#switchport trunk native vlan 1001  
SW2(config)#do sh int trunk


嗯？為什麼SW2這邊，沒有30  
Port        Vlans allowed and active in management domain  
Gig0/1      10

所以要把g0/1 port 加到VLAN30嗎？等一下看答案怎麼做......  
// 居然是直接在SW2的g0/1介面 下指令vlan 30，直接加一個vlan就好.....

SW2(config-if-range)#int g0/1  
SW2(config-if)#switchport mode access  
SW2(config-if)#switchport access vlan 30  
SW2(config-if)#do sh vl br

哦 不對 看起來就是有問題 又用no指令把g0/1從vlan30拿掉  
SW2(config)#  
%CDP-4-NATIVE_VLAN_MISMATCH: Native VLAN mismatch discovered on GigabitEthernet0/1 (30), with SW1 GigabitEthernet0/1 (1001).


然後active這邊30跑出來了？？？  
Port        Vlans allowed and active in management domain  
Gig0/1      10,30



// 先看第三題了  
3. Configure the connection between SW2 and R1 using 'router on a stick'.  
     Assign the last usable address of each subnet to R1's subinterfaces.






// 看答案才知道我這題漏掉了一個重要的東西  
// R1 == SW2 用router on a stick方法連接  
// 除了要在router端切子介面與設定IP外  
// SW2也必須去設定trunk port


int g0/2  
sw mode trunk  
sw trunk allowed vlan 10,20,30  
sw trunk native vlan 1001





要設定子介面需要去router設定  
需要各個子網路gateway的IP與遮罩

vlan10  
10.0.0.64 255.255.255.192

vlan20  
10.0.0.126 255.255.255.192

vlan30  
10.0.0.190 255.255.255.192


算一下遮罩  
10.0.0.(0)  
([00]000000)  
把網路部分設成1  
([11]000000) = 192


開始設定  
R1(config)#int g0/0  
R1(config-if)#no shut


子介面1  
R1(config-if)#interface g0/0.10  
R1(config-if)#encapsulation dot1q 10// 從這個介面進來 要打標籤10  
R1(config-if)#ip address 10.0.0.0 255.255.255.192

欸 有問題欸 出現bad mask錯誤訊息？？？ 啊 這邊要填gateway IP啦 遮罩沒錯  
他這個錯誤訊息也不是很準  
R1(config-subif)#ip address 10.0.0.0 255.255.255.192  
Bad mask /26 for address 10.0.0.0

所以應該是  
ip address 10.0.0.62 255.255.255.192


子介面2  
R1(config-if)#interface g0/0.20  
R1(config-if)#encapsulation dot1q 20  
R1(config-if)#ip address 10.0.0.126 255.255.255.192  
R1(config-subif)#do sh ip int br


子介面3  
R1(config-if)#interface g0/0.30  
R1(config-if)#encapsulation dot1q 30  
R1(config-if)#ip address 10.0.0.190 255.255.255.192  
R1(config-subif)#do sh ip int br

// 第四題  
4. Test connectivity by pinging between PCs.  All PCs should be able to reach each other.

我第二題就怪怪的 我猜可能會有問題  
先在PC1 ping PC3  
ping 10.0.0.129

果然ping失敗  
Ping statistics for 10.0.0.129:  
    Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),



