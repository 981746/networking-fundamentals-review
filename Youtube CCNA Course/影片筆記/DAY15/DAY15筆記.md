老師先對上次影片最後的練習做解答

Quiz Question2  
what subnet does host 172.21.111.201/20 belong to?

解答

我之前作答寫  
把不是borrow的bit設成0  
01101 000 00000000  // 我這裡怎麼把borrow bit寫成5個bit 應該是4個bit 不小心寫錯了  
所以最後答案算錯....

應該是這樣才對  
0110 0000 00000000  
2^5 + 2^6 = 32+64 = 96

答案是172.21.96.0/20


Quiz Question3  
what is the broadcast address of the network 192.168.91.78/26 belongs to?   
原來這題直接把borrow bits之後的可動的bits設成1就可以了  
因為borrow bits的部分 本來是host 那被用來當成子網路的網路的部分

那給我們一個IP  
prefix length其實已經包含網路的部分+borrow bits子網路部分  
然後borrow bits之後的bits 代表的是hosts

那給我們一個IP 前面網路(網路+子網路)的部分已經確定  
那讓hosts的部分為最大值 那就等於是broadcastIP了  
所以就是在host portion的binary pattern 設成1就會是最大值

192.168.91.(78)

[]代表borrow bits

([01] 001110)  
([01] 111111) = 127  
答案是192.168.91.127


Quiz Question4  
you divide the 172.16.0.0/16 network into 4 subnets of equal size.  
identify the network and broadcast address of the second subnet  
// 這一題那時候我沒寫 現在來寫

2^2 = 4  
需要兩個borrow bits

172.16.(0.0)  
([00]000000 00000000)


borrow bits代表子網路：  
00  
01→第二個子網路的borrow bits的pattern  
10  
11

要找廣播IP，所以把host bits都設成1  
[01]111111 11111111  
= 127      255  
(127.255)

答案是127.16.127.255// 欸看答案發現還要找網路IP 少找一個


// 現在來找question 4的第二組子網路的IP地址

剛剛找到第二個子網路的borrow bits的pattern  
就把接下來的hosts bits設成0 就是網路IP了  
[01]000000 00000000  
01000000 00000000  
= 64     0  
第二個子網路的網路IP是127.16.64.0


上次練習都解答完了  
現在要進入切Class A的子網路  
https://youtu.be/z-JqCedc9EI?si=sFmcQLx7Gh79mC9W&t=351


You have been given the 10.0.0.0/8 network.  
You must create 2000 subnets which will be distributed to various enterprises.  
What prefix length must you use?A：/19  
How many host addresses(usable addresses)will be in each subnet?A：8190  

要切2000個子網路  
那要borrow 多少個bits?  
2^8 = 256  
2^9 = 512  
2^10 = 1024  
2^11 = 2048

要借11個btis  
11 + 8 = 19  
所以prefix length會是/19  
hosts 部分的bits是32 - 19 = 13  
2^13 - 2 = 8192 - 2 = 8190  
所以每個subnet有8190 usable addresses


PC1 has an IP address of 10.217.182.223/11

Identify the following for PC1's subnet:  
(1)Network address   A：10.192.0.0  
(2)Broadcast address A：10.223.255.255  
(3)First usable address A：10.192.0.1  
(4)Last usable address  A：10.223.255.254  
(5)Number of host(usable) address A：2,097,150


作答(1)  
(10.217).182.223  
(00001010 11011001) xxxxxxxx xxxxxxxx  
[]代表borrow bits

(00001010 [110]11001) xxxxxxxx xxxxxxxx

現在把host部分設成0  
(00001010 [110]00000) 00000000 00000000  
= 10       192        0        0  
10.192.0.0

作答(2)  
找廣播IP  
現在把host部分設成1  
(00001010 [110]11111) 11111111 11111111  
=10       223         255      255  
10.223.255.255

作答(3)  
10.192.0.1

作答(4)  
10.223.255.254

作答(5)  
32 - 11 = 21  
2^21 - 2 = 2,097,150

現在談VLSM  
https://youtu.be/z-JqCedc9EI?si=2X2IVf38Kjl2Mn06&t=624


VLSM(Variable-Length Subnet Masks)

例子 要切5個子網路  
192.168.1.0/24  

Tokyo LAN A 110 hosts  
Tokyo LAN B 8 hosts  
point-to-point connection  
Toronto LAN A 29 hosts  
Toronto LAN B 45 hosts

如果是用FLSM(variable length subnet mask)方法  
我們需要借3個bits來切子網路 會切成8份 然後剩下3個子網路沒有使用到  
2^5 = 32 並且每個子網路只有32個hosts可用  
這樣Tokyo LAN A與Toronto LAN B根本不夠用

VLSM - Steps  
(1) Assign the largest subnet at the start of the address space  
(2) Assign the second-largest subnet after it  
(3) Repeat the process until all the subnets have been assigned


所以切網路的順序為  
Tokyo LAN A 110 hosts        →1  
Tokyo LAN B 8 hosts          →4  
point-to-point connection    →5  
Toronto LAN A 29 hosts       →3  
Toronto LAN B 45 hosts       →2  



step1 Tokyo LAN A 110 hosts  
Network address A：192.168.1.0/25  
Broadcast address A：129.168.1.127/25  
First usable address A：192.168.1.1/25  
Last usable address A：129.168.1.126/25  
Total number of usable hosts addresses：A：126



192.168.1.0/24

2^7 - 2 = 128 - 2 = 126  
所以host portion要有7個bits  
代表32 - 7 = 25  
/25  
要borrow 一個bit

192.168.1.(0)  
[]代表borrow bits  
([0]0000000)  
hosts部分設成0，就是網路地址，那他給我們的這個IP就是網路地址了

廣播IP，把hosts部分設成1

([0]1111111)  
01111111 = 127  
129.168.1.127是廣播IP

Tokyo LAN A 110 hosts       切出來的網路IP為192.1.0.0/25     廣播IP為192.168.1.127  
Tokyo LAN B 8 hosts                 
point-to-point connection  
Toronto LAN A 29 hosts  
Toronto LAN B 45 hosts       網路IP為192.168.1.128  


192.168.1.127 + 1個地址就是下一個網路的network IP  
192.168.1.128 is the network address of Tokyo LAN B  
需要45個hosts  
那Tokyo LAN B的prefix length應該是甚麼呢？


一樣去找以下的值  
Network address 192.168.1.128/???     A：192.168.1.128/26  
Broadcast address                     A：192.168.1.191/26  
First usable address                  A：192.168.1.129/26  
Last usable address                   A：192.168.1.190/26                  
Total number of usable hosts addresses  A：2^6 - 2 = 62 

Tokyo LAN B需要45個hosts，再加上網路IP與廣播IP，就是47個hosts

2^6 = 64  
hosts portion需要6個bits  
32 - 6 = 26  
所以prefix length是26

現在找廣播IP，把hosts portion設成1

192.168.1.(128)/26  
[]代表borrow bits  
[10]000000  
10111111 = 191  
192.168.1.191為廣播IP


所以目前的情況是  
Tokyo LAN A 110 hosts 切好了  
Tokyo LAN B 8 hosts  
point-to-point connection  
Toronto LAN A 29 hosts  
Toronto LAN B 45 hosts 切好了

下一個切Toronto LAN A 29 hosts  
// 有點懶得寫 照一樣的步驟繼續找  
// 由hosts數量的大小 由大到小去切子網路

切切切 切到最後一個point-to-point connection  
那之前提過可以用/30 或/31 來切point-to-point的網路  
但是老師說 在CCNA考試建議是使用/30來切


切子網路的相關練習網站  
http://www.subnettingquestions.com  
https://subnetting.org  
*https://subnettingpractice.com// 老師喜歡這個網站 說有很多比較挑戰的題目

作業  
每天都到此網站做一題切子網路的題目 至少持續一個禮拜