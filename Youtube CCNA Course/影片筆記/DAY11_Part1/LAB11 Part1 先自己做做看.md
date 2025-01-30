LAB11 Part1 先自己做做看
// 沒有ping成功XD

All devices have NO pre-configurations:  

1. Configure the PCs and routers according to the network diagram (hostnames, IP addresses, etc.)  
    Remember to configure the gateway on the PCs.
    (You don't have to configure the switches)

欸 PC的gateway怎麼設定啊？  
先觀察一下這個網路長怎樣：  
(1)有兩台PC，分別在兩端  
(2)有三台router  
(3)有兩台switch，分別在兩端

那我們：  
1.先去改R1 R2 R3 SW1 SW2的hostname  
以SW1為例，指令是Switch(config)#hostname SW1

2.先設定R1 R2 R3的各個介面的IP地址

R1有兩個介面  
g0/0 192.168.12.1 255.255.255.0  
g0/1 192.168.1.254 255.255.255.0

使用ip address指令去為兩個介面新增IP地址

以g0/0為例就好  
R1(config)#do show ip int br  
R1(config)#int g0/1  
R1(config-if)#ip address 192.168.1.254 255.255.255.0  
R1(config)#no shut

R1(config-if)#do show ip int br  
// 所以R1與SW1是通的，R1與R2的protocol欄位是down，還沒通欸  
Interface              IP-Address      OK? Method Status                Protocol   
GigabitEthernet0/0     192.168.12.1    YES manual up                    down  
GigabitEthernet0/1     192.168.1.254   YES manual up                    up  
GigabitEthernet0/2     unassigned      YES unset  administratively down down  
Vlan1                  unassigned      YES unset  administratively down down 


R2有兩個介面  
g0/0 192.168.12.2 255.255.255.0  
g0/1 192.168.13.2 255.255.255.0

以R2的g0/0為例就好  
R2>sh ip int br  
R2(config)#int g0/0  
R2(config-if)#ip add 192.168.12.2 255.255.255.0  
R2(config-if)#no shut  
R2(config-if)#do show ip int br

R2(config-if)#do show ip int br  
// R2與R1現在通了 狀態是up and up  
// R2與R3還沒通  狀態是up and down  
Interface              IP-Address      OK? Method Status                Protocol  
GigabitEthernet0/0     192.168.12.2    YES manual up                    up  
GigabitEthernet0/1     192.168.13.2    YES manual up                    down  
GigabitEthernet0/2     unassigned      YES unset  administratively down down  
Vlan1                  unassigned      YES unset  administratively down down 


R3有兩個介面  
g0/0 192.168.13.3 255.255.255.0  
g0/1 192.168.3.254 255.255.255.0  
指令不多寫了，直接看設定好的結果  
R3(config-if)#do sh ip int br  
// 現在R3與R2也通了  
Interface              IP-Address      OK? Method Status                Protocol  
GigabitEthernet0/0     192.168.13.3    YES manual up                    up  
GigabitEthernet0/1     192.168.3.254   YES manual up                    up  
GigabitEthernet0/2     unassigned      YES unset  administratively down down  
Vlan1                  unassigned      YES unset  administratively down down 



2. Configure static routes on the routers to enable PC1 to successfully ping PC2.  

這題是  
如果PC1要ping PC2，那它們之間的連接的相關設備的static route怎麼設定？

設定static route的指令是R1(config)# ip route ip-address netmask next-hop  

先看R1  
R1 PC1→PC2要通 目標網路192.168.3.0 255.255.255.0 next-hop192.168.12.0// 欸next填錯了  
R1 PC2→PC1要通 已直接連接PC1網路 不用設定  

再看R2  
R2 PC1→PC2要通 目標網路192.168.3.0 255.255.255.0 next-hop192.168.13.3  
R2 PC2→PC1要通 目標網路192.168.1.0 255.255.255.0 next-hop192.168.12.1  

三看R3  
R3 PC1→PC2要通 已直接連接PC2網路 不用設定  
R3 PC2→PC1要通 目標網路192.168.1.0 255.255.255.0 next-hop192.168.13.2  


照著以上的細節去設定router的route  
先設定R1  
R1(config)#ip route 192.168.3.0 255.255.255.0 192.168.12.0  
R1(config)#do show ip route  

.....上半部省略  
Gateway of last resort is not set  

     192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks  
C       192.168.1.0/24 is directly connected, GigabitEthernet0/1  
L       192.168.1.254/32 is directly connected, GigabitEthernet0/1  
S    192.168.3.0/24 [1/0] via 192.168.12.0  
     192.168.12.0/24 is variably subnetted, 2 subnets, 2 masks  
C       192.168.12.0/24 is directly connected, GigabitEthernet0/0  
L       192.168.12.1/32 is directly connected, GigabitEthernet0/0  

現在設定R2的static route  
R2(config)#ip route 192.168.3.0 255.255.255.0 192.168.13.3  
R2(config)#ip route 192.168.1.0 255.255.255.0  192.168.12.1// 這個第一次時 目標網路寫錯  

R2(config)#do sh ip route  
.....上半部省略  
Gateway of last resort is not set  

S    192.168.1.0/24 [1/0] via 192.168.12.1  
S    192.168.3.0/24 [1/0] via 192.168.13.3  
                    [1/0] via 192.168.12.1// 設定錯了 怎麼刪掉XD  
     192.168.12.0/24 is variably subnetted, 2 subnets, 2 masks  
C       192.168.12.0/24 is directly connected, GigabitEthernet0/0  
L       192.168.12.2/32 is directly connected, GigabitEthernet0/0  
     192.168.13.0/24 is variably subnetted, 2 subnets, 2 masks  
C       192.168.13.0/24 is directly connected, GigabitEthernet0/1  
L       192.168.13.2/32 is directly connected, GigabitEthernet0/1  
刪掉錯誤的ip route  
R2(config)#no ip route 192.168.3.0 255.255.255.0 192.168.12.1  
刪掉後再查一次routing table：  
......上半部省略  
Gateway of last resort is not set  

S    192.168.1.0/24 [1/0] via 192.168.12.1  
S    192.168.3.0/24 [1/0] via 192.168.13.3  
     192.168.12.0/24 is variably subnetted, 2 subnets, 2 masks  
C       192.168.12.0/24 is directly connected, GigabitEthernet0/0  
L       192.168.12.2/32 is directly connected, GigabitEthernet0/0  
     192.168.13.0/24 is variably subnetted, 2 subnets, 2 masks  
C       192.168.13.0/24 is directly connected, GigabitEthernet0/1  
L       192.168.13.2/32 is directly connected, GigabitEthernet0/1  



現在設定R3的static route  
R3(config)#ip route 192.168.1.0 255.255.255.0 192.168.13.2  
R3(config)#do sh ip route  
Gateway of last resort is not set  

S    192.168.1.0/24 [1/0] via 192.168.13.2  
     192.168.3.0/24 is variably subnetted, 2 subnets, 2 masks  
C       192.168.3.0/24 is directly connected, GigabitEthernet0/1  
L       192.168.3.254/32 is directly connected, GigabitEthernet0/1  
     192.168.13.0/24 is variably subnetted, 2 subnets, 2 masks  
C       192.168.13.0/24 is directly connected, GigabitEthernet0/0  
L       192.168.13.3/32 is directly connected, GigabitEthernet0/0  

static route都設定好後，去PC1 ping PC2  
ping 192.168.3.1


沒通欸.....欸我少設定了甚麼啊？會不會是因為我沒有去設定PC1的default gateway啊？  
所以是要在PC1的default gateway填寫0.0.0.0嗎？還是不行欸  

C:\>ping 192.168.3.1

Pinging 192.168.3.1 with 32 bytes of data:  

Request timed out.  
Request timed out.  
Request timed out.  
Request timed out.  

Ping statistics for 192.168.3.1:  
    Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),


還是是要設定default route？也是ping不成功欸....  
R1(config)# ip route 0.0.0.0 0.0.0.0 192.168.3.1
