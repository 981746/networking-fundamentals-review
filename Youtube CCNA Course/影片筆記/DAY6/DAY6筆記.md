PC1要傳資料到PC3

提供了  
Src IP  
Dest IP  
Src MAC  
但Dest MAC 是未知的，怎麼辦？ARP(Address resolution protocol)

PC1 sent ARP Request(broadcast)  
PC3 sent ARP Reply(unicast)

FFFF.FFFF.FFFF = broadcast MAC address

ARP table  
PC1從arp request  
PC3回arp reply後  
PC1的ARP table就有了PC3的IP與MAC的對應

GNS3是 IOS系統的模擬器  
GNS3本身不用錢 但IOS系統要  
GNS3可以搭配wireshark使用

ping來衡量round-trip time  
舉例PC1→PC3→再回到PC1的時間

ping  ICMP echo request並不廣播  所以arp一定要先使用  
      ICMP echo reply

指令：  
ping IP地址

為什麼第一個ping失敗了  
那是因為arp  
因為PC1並不知道目的設備的MAC地址  
所以要用arp 所以第一個ping失敗了

在cisco的priviledge exec mode  
的指令是show arp

show mac address-table

Vlan (virtaul local area network)

aging 時間到dynamic 地址會被移除  
或手動移除dynamic地址  
clear mac address-table dynamic

手動刪除某一個dynamic mac地址  
clear mac address-table dynamic address 0c2f.b011.9d00

手動刪除以某個連接介面來決定刪除的dynamic mac地址  
clear mac address-table dynamic address interface Gi0/0

ping 192.168.1.3 size 36 //傳36bytes的資料  
//但一個frame的data大小至少要46 bytes  
// 46 - 36 = 10 bytes

每個16進制的數值是4bit  
所以2digit 等於8bit  
所以每兩個digit等於一個byte  
去數0，可以發現真的是10個bytes

當一個frame裡的type的值為0x0806 代表的是ARP