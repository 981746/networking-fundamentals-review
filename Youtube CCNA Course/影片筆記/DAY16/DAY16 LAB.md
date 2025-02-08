1. Configure the correct IP address/subnet mask on each PC.  
    Set the gateway address as the LAST USABLE address of the subnet.  

2. Make three connections between R1 and SW1.  
    Configure one interface on R1 for each VLAN.  
    Make sure the IP addresses are the gateway address you configured on the PCs.


第1與第2題一起做  
好像要先算子網路   
去找到last usable IP給與LAN對接的router  
PC的IP與gateway都要設定


一、先看VLAN10這個子網路區塊

10.0.0.(0)/26  
([00]000000)  
host portion是6個bits  
2^6 = 64  
number of usable addresses = 2^6 - 2 = 64 - 2 = 62

找廣播IP  
([00]111111)  
2^0 * 1 + 2^1*1 + 2^2 *1 + 2^3*1 + 2^4*1 + 2^5*1 =  
1 + 2 +4 +8 + 16 +32 = 63  
10.0.0.63→廣播IP

所以last usable address = 10.0.0.62 →gateway IP

找遮罩  
[11]000000 = 2^6 + 2^7 = 64 + 128 = 192  
255.255.255.192→遮罩

現在可以去設定  
PC1 IP 題目有給10.0.0.1 遮罩是255.255.255.192  
PC1 gateway以last usable address設定：10.0.0.62

PC2 IP 題目有給10.0.0.2 遮罩是255.255.255.192  
PC2 gateway以last usable address設定：10.0.0.62

去設定R1的g0/0介面為剛剛幫PC設定的gateway IP  
int g0/0  
ip add 10.0.0.62 255.255.255.192  
no shut


二、現在看VLAN20  
10.0.0.64/26

先找廣播IP  
10.0.0.(64)  
([01]000000)  
把host portion設成1就是廣播IP  
([01]111111) =   
64 + 2^5*1 + 2^4 *1 + 2^3*1 + 2^2*1 + 2^1*1 + 2^0 *1 =  
64 + 32 + 16 + 8 + 4 + 2 +1 = 127  
10.0.0.127→廣播IP

所以last usable address = 10.0.0.126→gateway IP

現在去設定  
PC3 IP 10.0.0.65 遮罩255.255.255.192  
PC3 gateway是10.0.0.126

PC4 IP 10.0.0.66 遮罩255.255.255.192  
PC4 gateway是10.0.0.126

去設定R1的G0/1介面  
int g0/1  
ip add 10.0.0.126 255.255.255.192  
no shut

三、現在看VLAN30  
10.0.0.128/26


10.0.0.(128)  
([10]000000)

找廣播IP  
([10]111111) = 191  
10.0.0.191→廣播IP


Last usable address:10.0.0.190→gateway IP


現在去設定  
PC5 IP 10.0.0.129 遮罩255.255.255.192  
gateway：10.0.0.190

PC6 IP 10.0.0.130 遮罩255.255.255.192  
gateway：10.0.0.190

現在去設定R1的g0/2介面  
int g0/2  
ip add 10.0.0.190 255.255.255.192  
no shut



3. Configure SW1's interfaces in the proper VLANs.  
    Remember the interfaces that connect to R1!  
    Name the VLANs  
     (Engeering, HR, Sales)


SW1>sh vlan br  

```
VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Gig0/1, Gig1/1, Gig2/1, Fa3/1
                                                Fa4/1, Fa5/1, Fa6/1, Fa7/1
                                                Fa8/1, Fa9/1
```

先做VLAN10  
interface range g0/1,f3/1,f4/1  
switchport mode access  
switchport access vlan 10  
vlan 10  
name Engeering


再做VLAN20  
interface range Gig1/1,f5/1,f6/1  
switchport mode access  
switchport access vlan 20  
vlan 20  
name HR

最後VLAN30  
interface range Gig2/1, Fa7/1, Fa8/1  
switchport mode access  
switchport access vlan 30  
vlan 30  
name Sales

4. Ping between the PCs to check connectivity.  
    Send a broadcast ping from a PC (ping the subnet broadcast address),  
     and see which PCs devices receive the broadcast
      (use Packet Tracer's 'Simulation Mode')

// 總覺得我這邊的packet tracer的行為跟老師的演示不太一樣 怪怪


