CIDR(Classless Inter-Domain Routing)  

IANA(Internet Assign Numbers Authority)  
把IPv4地址指派給公司  
大公司可能會被分配到Class A或B network  
而小公司可能會被分配Class C network// Class C網路裡的地址 只有256個  
他說這樣會浪費很多地址！

兩台router間的是point-to-point network  
假設這個point-to-point network用了一個Class C網路  
但只使用了四個地址：  
(1)網路本身的IP、(2)廣播IP、(3)在此網路的R1介面IP、(4)在此網路的R2介面IP

這樣浪費了256 - 4= 252  
浪費了252個IP地址

IETF在1993年引進了CIDR去替換掉classful addressing system

CIDR讓我們可以用不同的prefix length  
在這個例子中本來是/24  
那可以改成/25 或/26 或27 或/28...../31 或/32等等等

/25網路裡有幾個IP地址  
32 - 25 = 7  
2^7 - 2= 128 - 2 = 126  
...  
...  

/30  
32 - 30 =   
2^ - 2 = 2// 這一個選擇最好 還剩兩個地址剛好可以給R1與R2


/31→老師說這個會怪怪的  
32 - 31 = 1  
2^1 - 2 = 0// 阿 扣掉網路IP與廣播IP  就沒地址了  

/32  
32 - 32 = 0  
這樣無法去計算地址


注意一下不同的prefix length的遮罩數值  
/26的遮罩是  
11111111.11111111.11111111.11000000  
255.255.255.192



所以203.0.113.0/30是Class C的subnet  
203.0.113.0  
203.0.113.1  
203.0.113.2  
203.0.113.3  
使用/30 這個prefix length 共有四個地址  
所以範圍是203.0.113.0~203.0.113.3

所以203.0.113.4 ~ 203.0.113.255就變成了開放的IP地址  
就可以被別的subnet去使用

欸 好奇怪  
剛剛說/31 扣掉網路IP與廣播IP後 應該是沒有其他IP地址可用了  
但現在又說  
point-to-point network是可以用這組/31 prefix length的  
那怎麼用？  
他說 point-to-point network不需要網路IP與廣播IP  
那就有空間assign給兩台router  
所以  
203.0.113.2~203.0.113.255這一整段就空出來 可以給其他的subnet用

所以  
point-to-point network要用/30 或/31都可以


CIDR notation像是/25 /26 /27這種記號  
CIDR notation對應的dotted decimal IP地址要記吧？  
總不可能考試在那邊手算吧XD


最後再一個例子  
4台switch 都接上R1  
每一台switch都有45個host

但是呢一個網路除了45個host外，還需要網路IP與廣播IP  
所以一個子網路需要47個IP地址

47 * 4 = 188  
總計需要188個地址

範例給我們的網路是192.168.1.0/24  
2^8 = 256  
這個網路有256個地址可用 沒問題

哦 居然是從/30 /29 /28 這樣慢慢找= =

/26  
2^6 - 2 = 62個可用地址  
然後這個例子剩下的部分當作業