### Cisco IOS 指令筆記匯總：
源於：
DAY4筆記(不同的權限與修改密碼)  
DAY4 LAB(改hostname)  
DAY6筆記(ARP and Dynamic Mac Address)  
DAY8筆記(進入interface設定模式、查看IP介面狀況、修改IP)  
DAY9筆記(查switch介面狀態、Speed、Duplex、interface range)


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
> Written with [StackEdit](https://stackedit.io/).


