1. Configure R1's hostname

	    A：
	    Router>en
	    Router#conf t
	    Router(config)#hostname R1

2. Use a 'show' command to view a list of R1's interfaces, their IP addresses, status, etc.

	    A：
	    R1#sh ip int br

3. Configure the appropriate IP addresses on R1's interfaces, and enable the interfaces
    Configure appropriate interface descriptions


		A：
		R1#conf t
		R1(config)#


		R1(config)#int g0/0
		R1(config-if)#ip add 15.255.255.254 255.0.0.0
		R1(config)#do sh ip int br


		R1(config)#int g0/1
		R1(config-if)#ip add 182.98.255.254 255.255.0.0
		R1(config-if)#do sh ip int br

		R1(config-if)#int g0/2
		R1(config-if)#ip add 201.191.20.254 255.255.255.0
		R1(config-if)#do sh ip int br


		R1(config-if)#int g0/0
		R1(config-if)#description ## to SW1 ##
		// 欸 怎麼查description  我的筆記好像抄錯= =
		// 沒抄錯！但我的packet tracer真的無法用 老師可能用的是別的模擬器？
		// 他在這個練習是直接在running-config看description

		R1(config)#int g0/1
		R1(config-if)#des ## to SW2 ##


		R1(config-if)#int g0/2
		R1(config-if)#des ## to SW3 ##


		訂正：
		每一個interface還需要下no shutdown指令，我忘記了



4. Use a 'show' command to verify R1's interfaces again.

	    A：
	    R1#do sh ip int br

5. View the running config to confirm the configuration changes, then save the config

	    A：
	    R1(config-if)#do sh run 
	    R1#write// 原來一定要在priviledge exec 才能寫入設定檔.....


6. Configure the IP addresses of PC1, PC2, and PC3
   (Watch the video to learn how to do this in Packet Tracer)

	    A：
	    PC1→Config Tab→FastEthernet0
	    →IP Configuration區塊的IPv4 Address填寫15.0.0.0
	    subnet mask 點一下會自動填
	    按叉叉離開





7. Ping from PC1 to PC2 and PC3 to test connectivity

	    A：
	    C:\>ping 182.98.0.1
	    C:\>ping 201.191.20.1


> Written with [StackEdit](https://stackedit.io/).