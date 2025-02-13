前面都說switch是layer2  
但是很多moden switch layer3 capable as well

remove from ccna topic list:// 但還是會介紹 在下一集影片才介紹  
DTP(Dynamic Trunking Protocol)  
VTP(VLAN Trunking Protocol)

現在開始談native vlan on a router(ROAS)  
之前說因為安全問題  
native vlan應該要設定成unused vlan

那如果想要用native vlan要怎麼用呢？  
untagged→所以比較有效率  
allowed device to send more frames per second

之前在  
sw1，把g/0介面的native vlan設定成1001  
sw2，把g0/0 與 g0/1介面的native vlan設定成1001  
現在把native vlan設定成10

sw1這邊  
int g0/0  
switchport trunk native vlan 10

sw2這邊  
int g0/0  
switchport trunk native vlan 10  
int g0/1  
switchport trunk native vlan 10


兩種在router設定native vlan的方法：  
1.在router的子介面(邏輯上的切分介面)，下指令encapsulation dot1q [vlan-id] native  
R1(config)#int g0/0.10  
R1(config-subif)#encapsulation dot1q 10 native   
他說 這次的topology是沿用上堂課的  
所以子介面的IP是設定好的了  
// 所以這次改了甚麼？  
// 之前是  
// SW1 == SW1 trunk port(vlan10 vlan30)  
// SW2 == R1  trunk port(vlan10 vlan20 vlan30)  
// SW1 g0/0、SW2 g0/0、SW3 g0/1的native vlan是1001  
// 這是把native vlan從1001改成10  
// 所以意思是vlan10的frame都會變成untagged frame嗎？  
// 然後vlan20 vlan30維持原樣

現在要用wireshark來看frame的tag狀況// vlan20的pc 傳資料→vlan10的pc  
首先要看的是vlan20→SW2→R1這一段的狀況  
因為vlan20不是native vlan  
所以這個frame會是tagged的狀態




2.在router的實體介面設定native vlan的IP


現在用wireshark看  
R1→SW2的狀況

因為目標是vlan10  
照理說tag會是vlan10，但是呢vlan10被設定成native vlan  
所以這次就沒有dot1q tag

現在看第二種設定native vlan的方法  
2.在router的實體介面設定native vlan的IP

下指令：  
R1(config)#no interface g0/0.10// 之前有去切子介面，現在把子介面還回去  
R1(config)#interface g0/0  
R1(config)#ip address 192.168.1.62 255.255.255.192

現在我們可以看一下running config的狀態  
// 所以是vlan 10就不需要子介面了 直接設定實體介面IP  
// vlan20 vlan30一樣要切子介面 與設定dot1q tag

interface GigabitEthernet0/0  
  ip address 192.168.1.62 255.255.255.192

interface GigabitEthernet0/0.20  
  encapsulation dot1q 20  
  ip address 192.168.1.126 255.255.255.192


interface GigabitEthernet0/0.30  
  encapsulation dot1q 30  
  ip address 192.168.1.190 255.255.255.192
  
  
再回到我們的網路圖  
有一台router、兩台layer2 switch

現在介紹layer3 switch的圖示  
也稱為multilayer switch

*這種switch的能力有switching 與 routing  
要注意的是在layer3層 運作

*所以我們可以在layer3 switch上設定IP  
而一般layer2 switch不行，因為layer2關注的是mac address

*還可以在layer 3 switch建立虛擬介面(for vlan)，然後再assign IP到那些介面

*還可以像router一樣，去建立route

*也可用來做inter-VLAN routing
他說  
SW == Router  
trunk port == router on a stick方法  
全部都塞在同一條線 會造成網路壅塞  
所以在大的網路 multilayer switch is a prefer method for inter vlan routing  

現在投影片標題是Inter-VLAN Routing via SVI

R1 == SW2(layer3)  
之間本來是多條vlan的線路  
現在改成  
point to point layer 3 link


本來如果SW2是layer switch，資料的傳遞路徑會是，pc→sw→router→sw→....  
就是不同的vlan傳遞資料 一定要經過router  
但是因為現在switch改成layer3 switch了  
layer3 switch有一個功能叫switch virtual interfaces

*SVIs(Switch Virtaul Interfaces)是一個虛擬介面 你可以assign IP  
*必須在PC端 去設定gateway為SVI，而不是router  
*如果用layer3 switch的話，不同的vlan傳遞資料，就只要經過layer3 switch，不再需要去router那邊轉一圈

所以SW2(layer 3)的SVI設定可以這樣寫：  
VLAN10: 192.168.1.62  
VLAN20: 192.168.1.126  
VLAN30: 192.168.1.190  
// 這些IP都是子網路的last usable address  
// 本來是設定給router的子介面  
// 現在改成設定在layer switch這邊  
// PC端的gateway其實不用改 因為數值是一樣的

所以資料的傳遞路徑會像這樣  
VLAN20 PC→SW2(layer3)→→SW1(layer2)→到達目標VLAN10 PC

SW2(layer3)現在有自己的routing table

網路圖又加上一朵雲 代表網際網路  
他說 如果資料要送到Internet 那會怎麼傳呢  
現在的default gateway是SW2(layer3)  
SW2(layer3) == R1，現在是point to point connection  
在SW2(layer3)設定一條default route，所以目標為LAN之外的IP，都會被送到R1  
那我們現在看怎麼設定？


SVI設定  
// 記得SVI預設是shutdown，所以記得要使用no shutdown打開SVI  
SW2(config)#interface vlan10  
SW2(config-if)#ip address 192.168.1.62 255.255.255.192  
SW2(config-if)#no shutdown

SW2(config)#interface vlan20  
SW2(config-if)#ip address 192.168.1.126 255.255.255.192  
SW2(config-if)#no shutdown

SW2(config)#interface vlan30  
SW2(config-if)#ip address 192.168.1.190 255.255.255.192  
SW2(config-if)#no shutdown


建立一個SVI 但是是不存在的VLAN，會發現狀態會是down down  
SW2(config-if)#interface vlan40  
SW2(config-if)#ip address 40.40.40.40 255.255.255.0  
SW2(config-if)#no shutdown  
SW2(config-if)#do sh ip int br  
......  
Vlan40   40.40.40.40   YES   manual   down    down  

那SVI的狀態是up and up的條件是甚麼？  
1.VLAN必須在switch上有紀錄  
2.  
如果是使用access port設定vlan，至少要有一個port是up and up狀態  
或是用trunk port必須允許vlan，而且也要是up and up狀態

3.vlan must not be shutdown  
vlan本身不能是shutdown狀態

4.SVI本身不能是shutdown狀態(預設為shutdown，所以要用no shut指令)

回到網路圖  
sw2 有連接兩個VLAN(10 and 20)  
所以vlan10 與 vlan20的SVI的狀態可以為up and up  
但是呢 sw2並沒有直接與vlan30連接，但是卻有trunk port g0/0  
這個trunk port允許了vlan30通過  
所以vlan30的SVI就可以為up and up狀態


SW2(config-if)#do show ip route  
可以看到我們設定的SVI的IP都出現在routing table裡了