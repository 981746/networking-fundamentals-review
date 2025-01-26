Cisco IOS 指令筆記匯總：//源於DAY4筆記、DAY4 LAB、DAY8筆記


#### DAY4筆記：

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

#### DAY4 LAB

 1. 改設備的hostname

		Router(config)#hostname R1

#### DAY8筆記：

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

> Written with [StackEdit](https://stackedit.io/).