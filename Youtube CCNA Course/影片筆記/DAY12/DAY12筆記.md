The life of a Packet

情境  
PC1要送資料到PC4  
PC1與PC4在不同的網路

而因為PC1與PC4在不同的網路  
所以PC1會先送一個ARP Request給與PC1網路連接的router(所謂的default gateway)，這裡是R1


ARP Request  
Src IP 192.168.1.1  
Dst IP 192.168.1.254 // Router IP 或是說這是default gateway  
Dst MAC ffff.ffff.ffff//此種類型的MAC地址代表要廣播 因為不知道目的地的MAC Address  
Src MAC 1111

在PC1網路裡與switch(SW1)連接的設備都會收到廣播(發送方除外)

R1收到ARP Request後，知道目標IP就是自己的G0/0介面的IP  
所以R1會送一個ARP reply給PC1，並且告訴PC1「自己(這裡是R1)的G0/0介面的MAC地址」  
這時PC1才知道default gateway的MAC Address  
並且傳送一個ehternet frame 這個frame會把default gateway的MAC Address做為目標MAC地址  
encapsulate在ethernet header裡

R1收到這個frame後 會remove ethernet header  
然後會去查他的routing table，去了解frame來的目標IP  
是否與routing table裡面的route有match  
查到了要用哪一個route後，就知道了目標網路與next-hop的IP(這個裡next-hop是R2的G0/0介面IP)  
所以如果我們要繼續傳這個packet，就要再把frame encapslate 包裝上next-hop的MAC地址  
但是R1不知道R2的G0/0介面的MAC地址  
所以 這個時候又要用到ARP(Address Resolution Protocol)  
R1的G0/0介面會送一個ARP Request 廣播出去  
R2的G0/0介面收到後，會回給R1 告訴其MAC地址

ARP Request  
Src IP 192.168.12.1  
Dst IP 192.168.12.2  
Dst MAC ffff.ffff.ffff  
Src MAC bbbb


OK，那現在R1知道R2的G0/0介面的MAC地址了  
就可以把這項資訊encapsulate在ethernet header裡 繼續傳遞資料 傳給R2  
那現在R2收到R1傳來的frame後，先de-encapsulate  
並且去routing table查next-hop的IP是甚麼  
那R2只知道next-hop的IP 不知道MAC Address  
所以此時又要送一個ARP Request(以廣播的方式 廣播R2所連接的網路上的設備)  
R4收到後 知道是自己的IP 然後回一個ARP Reply給R2

ARP Request  
Src IP 192.168.24.4  
Dst IP 192.168.24.f  
Dst MAC ffff.ffff.ffff  
Src MAC dddd


現在R2知道R4的G0/1介面的MAC Address  
所以R2有辦法發送frame給R4

R4收到後 remove the ehthernet header  
然後去查routing table  
找到match的那一條route，得知next hop與自己(R4)是直接連接的網路  
但是R4 不知道PC4的MAC地址  
所以 又要用到ARP  
R4要送一個ARP Requst(廣播)

ARP Request  
Src IP 192.168.4.254  
Dst IP 192.168.4.1  
Dst MAC ffff.ffff.ffff  
Src MAC fffe

PC4收到後 會回一個ARP Reply給R4 告訴自己的MAC地址  
R4收到後 才能以PC4的MAC地址 去傳遞資料給PC4  
到這邊才完成了PC1傳遞資料給PC4這件事！  


那這時 PC4要傳資料給PC1，且在statuc route也都設定好了的狀態  
那與先前的PC1→PC4有甚麼不一樣呢？  
因為先前PC1→PC4已經完成了多次ARP的處理  
所以這次PC4→PC1 就不需要再用到ARP了  
只是經過router時，還是一樣有de-encapsulate與encapsulate 然後到next-hop的流程