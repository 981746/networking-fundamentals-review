Subnet the 192.168.5.0/24 network to provide sufficient addressing for each LAN.  
(Also, the point-to-point connection between R1 and R2).  

Assign the first usable address to the PC in each LAN.  

Assign the last usable address to the router's interface in each LAN.  

Configure static routes on each router so that all PCs can ping eachother.  



*題目分析  
(1)LAN1 45 hosts  
(2)LAN2 64 hosts  
(3)LAN3 14 hosts  
(4)LAN4  9 hosts  
(5)point to point connection 需要2個地址(切/30 所以算4個)  
45 + 64 +14 + 9 +2 = 134個地址  
不對 各個LAN還需要網路IP與廣播IP吧  

45+2 +64+2 +14+2 +9+2 +2+2 = 144  
題目資訊整理：需要切5個子網路，總共需要144個hosts  


那prefix length為/24，host portion就是8個bits  
2^8 = 256 

假設borrow bits為  
2^2 = 4  
2^3 = 8  
借的bits為3的話，那host portion就剩下5個bits  
2^5 = 32  
所以用FLSM的方式去切網路 切8份 每個網路的hosts是32  
這樣根本就不夠用  
所以這題一定要以VLSM的方式去切子網路


*現在開始用VLSM的方式切子網路  
第一步 以hosts數量由大到小 決定切子網路的順序  
切子網路順序為(並列出所需hosts數量，包含網路IP與廣播IP)：  
LAN2 64 hosts →64 + 2 = 66 hosts  
LAN1 45 hosts →45 + 2 = 47 hosts  
LAN3 14 hosts →14 + 2 = 16 hosts  
LAN4  9 hosts → 9 + 2 = 11 hosts  
point to point connection →2+2 = 4 hosts


*切第一個子網路LAN2 66 hosts

求  
prefix length 192.168.5.0/???  A：/25  
network address A：192.168.5.0  
broadcast address A：192.168.5.127  
first usable address A：192.168.5.1  
last usable address  A：192.168.5.126  
Total number of usable hosts addresses A：128 - 2 = 126

2^5 = 32  
2^6 = 64  
2^7 = 128  
host portion要7個bits才有辦法容納66個hosts  
32 - 7 = 25  
所以prefix length是/25  

192.168.5.(0)  
([0]xxxxxxx)

找廣播地址 直接把host portion設成1  
([0]1111111) = 127  
所以廣播地址是192.168.5.127  


*切第二個子網路LAN1 47 hosts

求  
prefix length 192.168.5.0/???  A：/26  
network address A：192.168.5.128  
broadcast address A：192.168.5.191  
first usable address A：192.168.5.129  
last usable address  A：192.168.5.190  
Total number of usable hosts addresses A：64 - 2 = 62


從上一個子網路的結尾，也就是上一個子網路的廣播IP再加1個地址，就是第二個子網路的IP  
192.168.5.127 + 1 →192.168.5.128  
所以LAN1 network address為192.168.5.128

需要47hosts，那prefix length是？  
2^5 = 32  
2^6 = 64  
所以hosts portion所需要的bits為6  
32 - 6 = 26  
所以LAN1 的prefix length為/26

那現在來找廣播地址  
192.168.5.(128)  
先把()的部分轉成binary patter  
([10]000000)  
把host portion設成1  
([10]111111) = 191  
所以廣播地址是192.168.5.191


*切第三個子網路LAN3 16 hosts

求  
prefix length 192.168.5.0/???  A：/28  
network address  A：192.168.5.192  
broadcast address A：192.168.5.207  
first usable address A：192.168.5.193  
last usable address  A：192.168.5.206  
Total number of usable hosts addresses A：16 - 2 = 14

從上一個子網路廣播地址+1個地址，就是第三個子網路netwrok地址  
192.168.5.191 + 1 = 192.168.5.192

那現在來找host portion  
要容納16 hosts  
2^4 = 16  
所以hosts portion bits是4 bits  
32 - 4 = 28  
所以prefix length為/28

把()部分轉成bit pattern  
192.168.5.(192)  
[]代表borrow bits  
([1100]0000)  
把host portion設成1  
[1100]1111 = 207  
所以廣播IP是192.168.5.207



*切第四個子網路LAN4 11 hosts  
求  
prefix length 192.168.5.0/??? A：/28  
network address A：192.168.5.208  
broadcast address A：192.168.5.223  
first usable address A：192.168.5.209  
last usable address  A：192.168.5.222  
Total number of usable hosts addresses A：16 - 2 = 14

從上一個子網路的結尾+1個地址為第四個子網路的network IP  
192.168.5.207 + 1 = 192.168.5.208

現在來找prefix length  
要容納11 hosts  
2^4 = 16  
hosts portion是4 bits  
那32 - 4 = 28  
所以prefix length是/28


192.168.5.(208)  
把括號部分轉成bit pattern  
([1101]0000)

廣播地址是host portion設成1  
([1101]1111) = 223  
所以廣播地址是192.168.5.223

*切第五個子網路point to point connection 要容納4個地址  
求  
prefix length 192.168.5.0/???  A：30  
network address A：192.168.5.224  
broadcast address A：192.168.5.227  
first usable address  A：192.168.5.225  
last usable address  A：192.168.5.226  
Total number of usable hosts addresses 4 - 2 = 2  

從上一個子網路的結尾 廣播IP+1個地址 就是第五個子網路的network address  
192.168.5.223 + 1 = 192.168.5.224

現在找prefix length  
要容納4個地址  
2^2 =4  
所以host portion是2個bit  
32 - 2 = 30  
所以prefix length是30


192.168.5.(224)  
把()部分轉成bit pattern  
[]代表borrow bits  
([111000]00)

把host portion設成1即為廣播IP  
([111000]11) = 227  
所以廣播IP為192.168.5.227


////////////////////以上子網路切完 我們繼續看題目  
Assign the first usable address to the PC in each LAN.

LAN1  
first usable address A：192.168.5.129  
欸 還要填遮罩欸  
/26  
255.255.255.()  
([11]000000) = 192  
所以遮罩是255.255.255.192





LAN2  
first usable address A：192.168.5.1  
prefix length是25

找一下遮罩  
255.255.255.(128)  
([1]0000000) = 128



LAN3  
first usable address A：192.168.5.193  
prefix length是28

找一下遮罩  
255.255.255.()  
([1111]0000) = 240  
255.255.255.240


LAN4  
first usable address A：192.168.5.209  
prefix length是28

找一下遮罩  
255.255.255.()  
([1111]0000) = 240  
255.255.255.240

////////////assign完每一台PC的IP後，繼續看題目  
Assign the last usable address to the router's interface in each LAN.  
把last uable addres派給與LAN連接的Router介面

LAN1  
last usable address  A：192.168.5.190  
/26  
遮罩是255.255.255.192

所以要設定R1的g0/0介面的IP與遮罩  
R1(config)#int g0/0  
R1(config-if)#ip add 192.168.5.190 255.255.255.192



LAN2  
last usable address  A：192.168.5.126  
/25  
遮罩是255.255.255.128  
所以要設定R1的g0/1介面  
R1(config-if)#int g0/1  
R1(config-if)#ip address 192.168.5.126 255.255.255.128
記得no shutdown


LAN3  
last usable address  A：192.168.5.206  
/28  
遮罩是255.255.255.240  
所以要設定R2的g0/0介面的IP與遮罩  
ip add 192.168.5.206 255.255.255.240  
no shut


LAN4  
last usable address  A：192.168.5.222  
/28  
遮罩是255.255.255.240  
所以要設定R2的g0/1介面的IP與遮罩  
ip add 192.168.5.222 255.255.255.240  
no shut

///////////////////////router介面的IP與遮罩都設定好了 繼續看題目  
Configure static routes on each router so that all PCs can ping eachother.  

所以是以下要確定能雙向溝通  
(1)PC1 ping PC2 不用設定 兩個LAN中間與一台router相連  
(2)PC1 ping PC3  
(3)PC1 ping PC4  
(4)PC2 ping PC3  
(5)PC2 ping PC4  
(6)PC3 ping PC4 不用設定 兩個LAN中間與一台router相連

(1)PC1 ping PC2  
所以  
PC1→PC2 目標LAN2網路192.168.5.0 255.255.255.128 next-hop  
PC2→PC1 

/////router直接接switch 那就沒有next hop吧  
/////那不就是直接接通？  
所以PC1與PC2是直接接通的狀態吧  
我直接PC1 ping PC2看看  
在PC1的terminal：ping 192.168.5.1  
不行欸  
啊！我忘記設gateway  
PC都需要設定gateway


PC1的gateway是192.168.5.190  
PC2的gateway是192.168.5.126  
那PC1再ping看看PC2，ping 192.168.5.1，可以，ping得到  

那PC3與PC4也把gateway設定一下  

PC3 gateway:192.168.5.206  
PC4 gateway:192.168.5.222  

然後在PC3 ping PC4  
ping 192.168.5.209


////////////////////////////////因此要設定static route的有下列  
(2)PC1 ping PC3  
(3)PC1 ping PC4  
(4)PC2 ping PC3  
(5)PC2 ping PC4  


我們看(2)PC1 ping PC3  
PC1 →PC3 目標網路 192.168.5.192 255.255.255.240 next-hop是R2的G0/0/0介面192.168.5.226  
////欸 這邊是router 的point to point connection  
////我還沒有幫point to point connection的兩邊的介面設定IP欸  
////題目沒有叫我設定 所以要自己主動去設定？



R1的g0/0/0 設ip add 192.168.5.225 255.255.255.252 記得no shut  
R2的g0/0/0 設ip add 192.168.5.226 255.255.255.252 記得no shut  
/30 算一下遮罩  

192.168.5.(225)  
把()轉成bit pattern  
([111000]01)  
([111111]00) = 252  
遮罩是255.255.255.252



PC3 →PC1 目標網路192.168.5.128 255.255.255.192 next-hop R1的g0/0/0 192.168.5.225  


/////r1的routing table  
ip route 192.168.5.192 255.255.255.240 192.168.5.226

/////r2的routing table  
ip route 192.168.5.128 255.255.255.192 192.168.5.225

然後在PC1 ping PC3  
ping 192.168.5.193  
OK欸，可以通



現在看(3)PC1 ping PC4  
先在PC1 ping看看PC4，還不能通，因為還沒設定static route  
PC1→PC4 目標網路192.168.5.208 255.255.255.240 next-hop R2的g0/0/0 192.168.5.226  
PC4→PC1 目標網路192.168.5.128 255.255.255.192 next-hop R1的g0/0/0 192.168.5.225  

/////r1的routing table  
ip route 192.168.5.208 255.255.255.240 192.168.5.226


//////r2的routing table  
ip route 192.168.5.128 255.255.255.192 192.168.5.225

現在在PC1 ping PC4  
ping 192.168.5.209  
可以通了！



現在看(4)PC2 ping PC3  
PC2→PC3 目標網路192.168.5.192 255.255.255.240 next-hop R2的g0/0/0 192.168.5.226  

PC3→PC2 目標網路192.168.5.0 255.255.255.128 next-hop R1的g0/0/0 192.168.5.225  


////在r1的routing table  
ip route 192.168.5.192 255.255.255.240 192.168.5.226

////在r2的routing table  
ip route 192.168.5.0 255.255.255.128 192.168.5.225

在PC2 ping PC3  
ping 192.168.5.193  
可以通！



最後一個了(5)PC2 ping PC4  
PC2→PC4 目標網路192.168.5.208 255.255.255.240 next-hop R2的g0/0/0 192.168.5.226  
PC4→PC2 目標網路192.168.5.0 255.255.255.128 next-hop R1的g0/0/0 192.168.5.225  

////在r1的routing table  
ip route 192.168.5.208 255.255.255.240 192.168.5.226

/////在r2的routing table  
ip route 192.168.5.0 255.255.255.128 192.168.5.225

在PC2 pin PC4  
ping 192.168.5.209  
OK OK可以通！

