VLAN20 是在SW2，  
然後SW1與SW2並沒有VLAN20的連結

舉例  
VLAN20的一台PC 想送資料到VLAN10(connect with SW1)的PC  
那要先送資料到R1 也就是default gateway  
PC(VLAN20)→SW2→R1→回到SW2 且位在VLAN10橘色的線上→SW1→目標PC(VLAN10)

所以是  
Src VLAN的PC→SW→Router// 這一段所在的線路會在Source VLAN上
那  
Router→回SW→...→目標PC// 這一段所在的線路會在Target VLAN上

這樣讓我們可以在不同的VLAN傳資料  
router讓我們可以在不同的VLAN傳資料  
這樣叫做inter vlan routing


那在比較小型的網路中  
或許我們會用不同的網路介面去區分VLAN  
但是隨著網路的擴張  
我們的設備根本沒有那麼多介面讓我們去區分不同的VLAN  
所以我們可以用trunck port只用一個介面 去承接不同VLAN來的資料流

現在重新調整網路圖  
注意SW之間與Router的VLAN線路有做修改

這個例子是  
與SW2連接的VLAN10裡的PC想傳資料到  
與SW1連接的VLAN10裡的PC  
                 R1  
                 ↑  
pc(VLAN10)←SW1←SW2←pc(VLAN10)

現在有一個問題  
how did switch1 know which vlan traffic belongs to?  
SW1是怎麼知道這個traffic屬於哪個VLAN？  
SW1與SW2之間有兩條VLAN線(VLAN10、VLAN30)，怎麼知道是哪一個？  
答案是VLAN Tagging

Switches will tag all frames that they send over a trunk link.  
This allows the receiving switch to know which VLAN the frame belongs to.  
所以是從trunk link出去的時候就會被tag？  
所以trunk port又被稱為tagged port  
access port又被稱為untagged port  

有兩個主要的trunk protocols  
ISL(Inter-Switch Link)  
IEEE 802.1Q// 念為dot one Q


ISL是舊的協定  
IEEE 802.1Q是目前的標準

回到DAY5 看一下Ethernet Frame  
802.1Q的TAG會被塞在Source欄位與Type/Length欄位之間  
TAG為4 bytes length  
TAG包含兩個欄位TPID(Tag Protocol Identifier)與TCI(Tag Control Information)  
然後TCI又包含三個子欄位

TPID      + TCI(PCP 3bits、DEI 1bits、VID 12 bits)  
16bits

*我們先來看TPID欄位  
16bits(2 bytes) length  
這個欄位總是塞入值0x8100，表示這個frame是802.1Q tagged

*TCI的子欄位PCP(Priority Code Point)  
3 bits in length  
被CoS使用(Class of Service)// 在擁塞的網路去安排優先序

*TCI的子欄位DEI(Drop Eligible Indicator)  
1 bit in length  
用來表示當網路壅塞時，frame可以被drop掉

*TCI的子欄位VID(VLAN ID)  
12 bits in length  
用來識別這個frame屬於哪個VLAN  
2^12 = 4096種VLANs可能，值是0到4095  
但是0與4095被保留，不能使用  
所以真正可用的VLAN值為1-4094  
ISL舊的tag protocol協定也是一樣使用1-4094這個範圍的值


現在談VLAN Ranges  
https://youtu.be/Jl9OOzNaBDU?si=LgTilMkyXLTrgxqC&t=745

Normal VLANs:1-1005  
Extended VLANs:1006-4094  
有些舊的設備不能使用Extended VLAN range

pc(VLAN10)←SW1←(這邊出去，出去時tag)SW2(這邊到switch)←pc(VLAN10)  

Native VLAN是802.1Q的feature  
VALN1 預設是all trunk ports

如果frame是屬於native VLAN，switch就不會新增802.1Q tag  
會正常forward frame

當一個trunk port收到untagged frame  
他會假定這個frame屬於native vlan  
所以不同switch的native vlan必須一致(matches)

trunk configuration  
https://youtu.be/Jl9OOzNaBDU?si=RSs1RsRARNldwV46&t=967

把三個介面設定成trunk ports  
(1)把SW1的g0/0介面、SW2的(2)g0/0、(3)g0/1介面


SW1(config)#interface g0/0  
SW1(config-if)#switchport mode trunk  
結果這個指令被拒絕，顯示trunk encapsulation is Auto can not be configured to trunk mode.  
哦 要先手動說明trunk使用dot1q封裝標準，才能設定為trunk mode

SW1(config-if)#switchport trunk encapsulation dot1q  
SW1(config-if)#switchprot mode trunk

然後  
SW1#show interfaces trunk

Port    Vlans allowed on trunk  
Gi0/0   1-4094// 為了安全應該要限制哪個VLAN on the trunk

所以  
SW1(config)#int g0/0  
SW1(config)#switchport trunk allowed vlan 10,30// 哦 直接說明allowed的vlan有哪些  
SW1(config)#do sh int trunk

ADD指令  
SW1(config)#switchport trunk allowed vlan add 20// 這是把某一VLAN新增進allowed list  
SW1(config)#do sh int trunk

老師說  
但是他還沒真正去在switch上去create vlan20  
所以在Vlans allowed and active in management domain這一列 看不到20

目前vlan20用不到 所以移除掉  
SW1(config-if)#switchport trunk allowed vlan remove 20  
SW1(config-if)#do show int trunk

讓trunk port allow all vlan，這樣又回到預設值  
SW1(config-if)#switchport trunk allowed vlan all  
SW1(config-if)#do sh int trunk

except選項  
allow all vlans except the one you specify  
SW1(config-if)#switchprot trunk allowed valn except 1-5,10

none選項  
SW1(config-if)#switchport trunk allowed vlan none  
no vlans are allowed on the trunk

因為SW1連接的host只有VLAN10 與VLAN30的hosts  
所以SW1的trunk port只需要允許VLAN10與VLAN30  
要這樣做的原因是因為安全問題並且最好把native VLAN為1改成unused VLAN

(1)先allowed VLAN20與VLAN30  
SW1(config)#int g0/0  
SW1(config-if)#switchport trunk allowed vlan 10,30  
SW1(config-if)#do show interfaces trunk

(2)再把native VLAN為1改掉，改成1001  
SW1(config-if)#switchport trunk native vlan 1001  
SW1(config-if)#do show interfaces trunk

這時去SW1#show vlan brief的VLAN1、VLAN10、VLAN30就看不到g0/0這個介面  
雖然VLAN10、VLAN30都為trunk port允許的allowed VLAN  
因為SW1#show vlan brief指令，顯示的是access port的狀態  
應該要用SW1#show interfaces trunk



前面做的是SW1的trunk port的設定  
現在要做的是SW2的trunk port的設定  
SW2 g0/0:allowed vlan10 vlan30  
SW2 g0/1:allowed vlan10 vlan20 vlan30

指令// 有點麻煩欸 好幾條指令 這邊寫SW2 g0/0怎麼設定就好 太多了  
SW2(config)#int g0/0  
SW2(config)#switchport trunk encapsulation dot1q  
SW2(config)#switchport mode trunk  
SW2(config)#switchport trunk allowed vlan10,30  
SW2(config)#switchport trunk native vlan 100  
SW2(config)#do sh int trunk


那Switch的trunk port就設定好了，可是R1的某一介面也連結了好幾條不同VLAN的線路  
那R1要怎麼允許多個VLAN通過呢？Router on a Stick(ROAS)  
因為R1與S2只有一條實體的連接，這樣的形式叫做stick  
我們可以把一條實體的 邏輯上切分成三條路 讓VLAN通過  
R1 g0/0.10  
R1 g0/0.20  
R1 g0/0.30


現在要來設定讓router的某一介面在邏輯上切分成三條路(允許三個VLAN通過)  
https://youtu.be/Jl9OOzNaBDU?si=gztO8ePaLxlYug0v&t=1669

R1(config)#int g0/0  
R1(config-if)#no shutdown

子介面一  
R1(config-if)#interface g0/0.10  
R1(config-if)#encapsulation dot1q 10// 從這個介面進來 要打標籤10  
R1(config-if)#ip address 192.168.1.62 255.255.255.192  
// 邏輯上會切三條線 router會對應有三個介面 所以router這邊也要去設定子介面的IP

子介面二  
R1(config-if)#interface g0/0.20  
R1(config-if)#encapsulation dot1q 20  
R1(config-if)#ip address 192.168.1.126 255.255.255.192

子介面三  
R1(config-if)#interface g0/0.30  
R1(config-if)#encapsulation dot1q 30  
R1(config-if)#ip address 192.168.1.190 255.255.255.192

R1#sh ip int br  
會看到子介面也出現了，但實體介面本身的IP-Address為unassigned

R1#sh ip route  
查routing table  
可以看到子介面的IP作為local與conneted route在其中

給一個例子是從VLAN10傳資料到VLAN30  
聽起來是SW2到R1時，給一個VLAN10 TAG  
而R1又回給SW2時，給一個VLAN30 TAG，然後後續才有辦法走VLAN30的路

所以  
Router上一個interface 邏輯上切分成三條線，使用的方式叫router on a stick(ROAS)，子介面的方式

那switch上一個interface，也就是一個port，切分成三條線，使用的方式叫trunk port

Quiz  
第一題是甚麼意思  
我連題目都看不太懂  
好像是從一個port 出去，這個port是trunk port  
可是傳輸資料的frame又要是untagged的形式，又要是vlan10  
哦 那就把vlan10設定成native啊  
以上為DAY17的內容

