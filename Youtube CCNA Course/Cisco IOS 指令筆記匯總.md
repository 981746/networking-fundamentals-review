### Cisco IOS 指令匯總：
源於：  
DAY4筆記(不同的權限與修改密碼)  
DAY4 LAB(改hostname)  
DAY6筆記(ARP and Dynamic Mac Address)  
DAY8筆記(進入interface設定模式、查看IP介面狀況、修改IP)  
DAY9筆記(查switch介面狀態、Speed、Duplex、interface range)  
DAY11 Part1筆記(查看Router的routing table)  
DAY11 Part2筆記(Router設定static routes、default route)  
DAY11 Part2 LAB(查running-config目前已設定的ip route的設定值)  
DAY12 LAB(查MAC Address、改改MAC Address)  
DAY16筆記(一次設定多個介面、使其為access port、並指派給某一vlan、修改vlan名稱)  
DAY17筆記(在switch上設定trunk port與允許的VLANs、在router上的某一介面切子介面與區分VLAN相關設定，叫做Router On A Stick方法)  
DAY18筆記(設定native vlan的兩種方法、以SVI來處理inter-vlan routing)  
// DAY18筆記有一部份內容我搞丟了，所以很多指令是重新看影片整理的






#### DAY4筆記(不同的權限與修改密碼)：

預設自動進入user exec mode

 1. 進入priviledge exec mode

	    Router>enable
	或

	    Router>en

 2. 進入global configuration mode

	    Router#configure terminal
	或

	    Router#conf t

 3. 查詢可用的指令

	    Router>?

 4. 改密碼(password方式改密碼會以明碼紀錄)

	    Router(config)#enable password CCNA

 5. 加密密碼(type 7 比較不安全)

	    Router(config)#service password-encryption

 6. 改密碼(以md5方式)

	    Router(config)#enable secret Cisco

 7. 查詢running-config

	    Router#show running-config
	或

	    Router#sh run
	或
	
	    Router(config)#do sh run// 使用do指令 在global模式下查看running config

 8. 查詢startup-config

	    Router#show startup-config
	或
	
	    Router#show start

 9. 退出某一個權限角色
 
	    Router#exit
	    
	也可以直接退回到User EXEC Mode(LAB8提到的)

	    Router#end



10. 儲存設定檔的三種方法

		Router#write
		Router#write memory
		Router#copy running-config startup-config
        
11. 取消加密password(但是目前已加密的密碼不會被解密)

        no service password-encryption

#### DAY4 LAB(改hostname)

 1. 改設備的hostname

		Router(config)#hostname R1

#### DAY6筆記(ARP and Dynamic Mac Address)

 1. 在PC上去ping

	    ping 192.168.1.3

 2. 在PC上查看arp table
註：如果是在cisco的設備要進入priviledge exec mode，指令是show arp

	    arp -a

 3. 查看所有Mac Address
type欄位會有標明是dynamic or 其他類型

		SW1>show mac address-table

 4. 清除某一筆Dynamic Mac Address

		clear mac address-table dynamic address MAC地址

 5. 清除所有Dynamic Mac Address

	    clear mac address-table dynamic

 6. 以interface，清除Dynamic Mac Address

	    clear mac address-table dynamic interface G0/0

#### DAY8筆記(進入interface設定模式、查看IP介面狀況、修改IP)：

 1. 查詢router的「全部」IP介面狀況

	    R1#show ip interface brief
	    R1(config-if)#do sh ip int br
 2. 進入「某一」IP介面設定模式

		R1(config)#interface gigabitethernet 0/0
  	    R1(config)#int g0/0
		 
 3. 查詢某個IP介面的狀態

		R1#show interfaces g0/0
 4. 設定router的IP地址

		R1(config-if)#ip address 10.255.255.254 255.0.0.0
		R1(config-if)#ip add 172.16.255.254 255.255.0.0
 5. 把router從shutdown狀態啟用

		R1(config-if)#no shutdown
 6. 查看IP介面的description(我的packet tracer無法用 老師可能用的是別的模擬器？)

		R1#show interfaces description

 7. 新增某IP介面的description(必須先切到interface config mode)

	    R1#int g0/0
	    R1(config-if)#description ## to SW1 ##


#### DAY9筆記(查switch介面狀態、Speed、Duplex、interface range)

 1. 查看switch的介面狀態

	    show interfaces status

 2. 設定Speed

	    SW1(config-if)#speed 100

 3. 設定duplex

	    SW1(config-if)#duplex full

 4. 設定description，並且會出現在`show interfaces status`的name field

	    SW1(config-if)#description ## to R1 ##

 5. 一次設定多個介面，先進入interface range config模式，再進行設定。  
這個例子是一次設定多個介面的description，並且把多個介面shutdown

	    SW1(config)#interface range f0/5 - 12
		SW1(config-if-range)#description ## not in use ##  
		SW1(config-if-range)#shutdown
        

    
#### DAY11 Part1筆記(查看Router的routing table) 

 1. 查看R1的routing table
	(在設定好Router各個介面的IP後，當你下達no shutdown指令 ，每個介面都會自動加入兩個路徑到routing table，也就是local與connected路徑)

	    R1# show ip route

#### DAY11 Part2筆記(Router設定static routes、default route)

 1. router設定static routes有三種方法  
 (設定default route也是一樣用此方法)

	    R1(config)# ip route ip-address netmask next-hop
	    R1(config)# ip route ip-address netmask exit-interface
	    R1(config)# ip route ip-address netmask exit-interface next-hop     
	     
		例子：
		R3(config)# ip route 192.168.4.0 255.255.255.0 192.168.13.1
		R2(config)# ip route 192.168.1.0 255.255.255.0 g0/0
		R2(config)# ip route 192.168.4.0 255.255.255.0 g0/1 192.168.24.4
        
        設定default route：
        R1(config)# ip route 0.0.0.0 0.0.0.0 203.0.133.2

#### DAY11 Part2 LAB(查running-config目前已設定的ip route的設定值)

 1. 查running-config目前已設定的ip route的設定值

	    show running-config | include ip route


#### DAY12 LAB(查MAC Address、改改MAC Address)

 1. 查MAC Address指令 In Windows PC

	    ipconfig /all

 2. 查MAC Address指令 In Cisco Device's interface

	    show interfaces g0/0

 3. 修改router的MAC Address
0000.01aa.aaaa為想修改的MAC Address
此舉動會導致MAC Address與BIA不一致

	    mac-address 0000.01aa.aaaa

#### DAY16筆記(一次設定多個介面、使其為access port、並指派給某一vlan、修改vlan名稱)

 1. 一次設定多個介面、使其為access port、並指派給某一vlan

	    SW1(config)#interface range g1/0 - 3
	    SW1(config)#switchport mode access
	    SW1(config)#switchport acess vlan 10
	    SW1(config)#do show vlan brief

 2. 修改vlan名稱

	    SW1(config)#vlan 10// 好像是新增vlan，並切到vlan 10的意思。已經有新增過就直接切過去
	    SW1(config-vlan)#name ENGINEERING


#### DAY17筆記(在switch上設定trunk port與允許的VLANs、在router上的某一介面切子介面與區分VLAN相關設定，叫做Router On A Stick方法)

 1. 說明使用哪一種TAG標準// 設備在允許ISP與802.1Q的狀態下，要先說明用哪一個

	    SW1(config-if)#switchport trunk encapsulation dot1q

 2. 把port轉成trunk port

	    SW1(config-if)#switchprot mode trunk

 3. 直接說明allowed的vlan有哪些

	    SW1(config)#switchport trunk allowed vlan 10,30

 4. 以下為allowed vlan的相關選項：

		ADD VLAN選項
		SW1(config)#switchport trunk allowed vlan add 20

		移除VLAN選項
		SW1(config-if)#switchport trunk allowed vlan remove 20

		ALL選項會回到預設值(允許所有VLAN)
		SW1(config-if)#switchport trunk allowed vlan all

		Except選項(指定的VLAN外，全部加入)
		SW1(config-if)#switchprot trunk allowed valn except 1-5,10

		NONE選項(此trunk port不允許任何VLAN)
		SW1(config-if)#switchport trunk allowed vlan none

 5. 把Native VLAN改掉(預設為1)，改成1001

		SW1(config-if)#switchport trunk native vlan 1001
		SW1(config-if)#do show interfaces trunk

 6. 顯示trunk port狀態

		SW1#show interfaces trunk

 7. 在switch上，設定trunk port與允許的VLAN相關指令全部串在一起

		SW2(config)#int g0/0
		SW2(config)#switchport trunk encapsulation dot1q
		SW2(config)#switchport mode trunk
		SW2(config)#switchport trunk allowed vlan10,30
		SW2(config)#switchport trunk native vlan 1001
		SW2(config)#do sh int trunk

 8. router的某一介面在邏輯上切分成三條路(允許三個VLAN通過)(Router On A Stick方法)

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


#### DAY18筆記(設定native vlan的兩種方法、以SVI來處理inter-vlan routing)

 1. 在router設定native vlan有兩個方法：

	(0)預處理步驟：
	找到有設定trunk port的介面，設定native vlan為vlan10
	(本來的trunk port介面的native vlan為unused port 1001)
	
		switchport trunk native vlan 10

	(1)直接把子介面設為native
	指令格式：encapsulation dot1q [vlan-id] native

		R1(config)#int g0/0.10
		R1(config-subif)#encapsulation dot1q 10 native

	或
	(2)把子介面刪掉，把原先子介面的IP設定在實體介面上
	
		R1(config)#no interface g0/0.10
		R1(config)#interface g0/0
		R1(config)#ip address 192.168.1.62 255.255.255.192

 2. 以SVI來處理inter-vlan routing

	> 	(1)router拿掉子介面、介面回到預設值(因為上一集使用SW(trunk port) ==  Router (ROAS方法切子介面)  
	> 	(2)設定R1與SW2(layer3)的point to point connection，R1部分  
	> 	(3)設定R1與SW2(layer3)的point to point connection，SW2部分：  
	> 	先把switch介面回到預設值(因為上一集為trunk port)、再打開routing功能、轉換成routed port、設定IP  
	> 	(4)在layer3 switch，設定default route指向R1  
	> 	(5)SVI設定 - 在layer 3
	> switch直接新增各個虛擬的VLAN介面與設定IP

	(1)router拿掉子介面、介面回到預設值

	SW2(layer3) == R1
	架構圖中把SW2改成layer 3 switch，所以SW2與R1的連接現在變成point to point connection
	因為本來SW2(layer2) == R1
	R1這邊的介面有設定router on a stick，也就是有去切子介面
	所以現在要把子介面都拿掉，且把g0/0介面設定為預設值

		R1(config)#no interface g0/0.10
		R1(config)#no interface g0/0.20
		R1(config)#no interface g0/0.30
		R1(config)#default interface g0/0
		R1(config)#do sh ip int br

	(2)設定R1與SW2(layer3)的point to point connection，R1部分
	R1這邊介面的IP要設定為point to point connection網路的last usable address

		R1(config)#interface g0/0
		R1(config)#ip address 192.168.1.194 255.255.255.252
		R1(config)#do sh ip int br

	(3)設定R1與SW2(layer3)的point to point connection，SW2部分

	SW2的g0/1在上一課被設定為trunk與router對接
	現在要先回到初始設定
	以ip routing指令開啟switch的routing功能
	以no switchport指令讓port從layer 2 switchport變成layer3 routed port
	最後才是設定switch這邊的point to point connection的IP

		SW2(config)#default interface g0/1
		SW2(config)#ip routing
		SW2(config)#interface g0/1
		SW2(config)#no switchport
		SW2(config)#ip address 192.168.1.193 255.255.255.252
		SW2(config)#show interfaces status// 介面的VLAN欄位會變成routed

	(4)在layer3 switch，設定default route指向R1
	
		SW2(config)#ip route 0.0.0.0 0.0.0.0 192.168.1.194
		SW2(config)#do sh ip route

	(5)SVI設定 - 在layer 3 switch直接新增各個虛擬的VLAN介面與設定IP  
	以router處理inter-vlan routing 會在router切子介面，並且設定IP  
	但現在改成在layer 3 switch直接新增虛擬的VLAN介面與設定IP

		SW2(config)#interface vlan10
		SW2(config-if)#ip address 192.168.1.62 255.255.255.192
		SW2(config-if)#no shutdown

		SW2(config)#interface vlan20
		SW2(config-if)#ip address 192.168.1.126 255.255.255.192
		SW2(config-if)#no shutdown

		SW2(config)#interface vlan30
		SW2(config-if)#ip address 192.168.1.190 255.255.255.192
		SW2(config-if)#no shutdown

