#### 第一題

point to point connection要設定的IP為  
R1 g0/0 要設定成10.0.0.194  

SW2 g1/0/2 10.0.0.193

先查一下R1的介面狀況  
R1#sh ip int br  
Interface              IP-Address      OK? Method Status                Protocol   
GigabitEthernet0/0     unassigned      YES NVRAM  up                    up   
GigabitEthernet0/0.10  10.0.0.62       YES manual up                    up   
GigabitEthernet0/0.20  10.0.0.126      YES manual up                    up   
GigabitEthernet0/0.30  10.0.0.190      YES manual up                    up  

好的，我先把子介面都先刪掉  
no int g0/0.10  
no int g0/0.20  
no int g0/0.30  


然後設定R1部分的point to point connection  
int g0/0  
ip add 10.0.0.194 255.255.255.192


SW2部分的point to point connection  
(1)要先把trunk port 拿掉  
所以是要讓g1/0/2介面回到初始設定  
default int g1/0/2

(2)設定IP  
ip routing  
int g1/0/2  
no switchport  
ip add 10.0.0.193 255.255.255.192



現在要設定SW2的deaful route 指向R1的g0/0介面  
ip route 0.0.0.0 0.0.0.0 10.0.0.194


#### 第二題  
每個子網路的last usable address為？手動算好像有點麻煩  
VLAN10  
10.0.0.(0)/26  
([00]111111) = 63  
10.0.0.63是廣播IP  
last usable address = 10.0.0.62

剛剛R1的子介面那邊就有寫了  
10.0.0.62  
0.0.0.126  
10.0.0.190  

現在在SW2新增SVI  
int vlan10   
ip add 10.0.0.62 255.255.255.192

int vlan20  
ip add 10.0.0.126 255.255.255.192

int vlan30  
ip add 10.0.0.190 255.255.255.192


#### 第三題 測試各個VLAN的連接狀況  
我從PC7去ping VLAN10 VLAN20 VLAN30裡的PC  
都OK

#### 第四題測試與Internet的連接狀況  
ping 1.1.1.1  
也OK
