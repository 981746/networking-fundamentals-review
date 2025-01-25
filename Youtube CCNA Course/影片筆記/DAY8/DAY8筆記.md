A類IP地址的第一個octet 是0-127  
\n但是因為0與127是被reserved 所以有些人會說是1-126
\n
\nMaximum Hosts per Network
\n192.168.1.0/24→這是Class C地址
\n
\n192.168.1.0/24 到 192.168.1.255/24
\n0到255是256種可能
\n其實就是最後一個btye，2^8= 256種可能
\n但是最後一個byte的decimal值是0與255會被分別保留給network與broadcast使用
\n所以只有256 - 2 = 254種可能
\n
\n現在
\n172.16.0.0/16→這是ClassB地址 到 172.16.255.255/16
\n後面兩個byte代表的是host地址的可能數量
\n2bytes = 16bits = 2^16 = 65,536
\n但是host portion全是0時與全是1時，會被分別保留給network與broadcast使用
\n也就是後面兩個byte的pattern的十進位是.0.0與.255.255會被保留
\n65,536 - 2 = 65,534
\nMaximum hosts per network = 2^16 -2 = 65,534
\n
\n再來
\n10.0.0.0/8 →這是Class A地址
\n後面三個byte代表的是host portion
\n當host portion全是0時與全是1時，會被分別保留給network與broadcast使用
\n也就是後面三個byte的pattern的十進位是.0.0.0與.255.255.255會被保留
\n
\n2^24 = 16,777,216
\nMaximum hosts per network = 2^24 -2 = 16,777,214
\n
\nFirst/Last Usable Address(第一/最後 可用地址)
\n192.168.1.0/24 到 192.168.1.255/24 →這是Class C地址
\n所以host portion是一個byte，8個bits
\n
\n00000000 →增加1      0000001
\n         十進制       1
\n所以就是192.168.1.1
\n這個叫first usable address
\n
\n
\n11111111(廣播地址)  減1 →   11111110
\n                    十進制   254
\n所以就是192.168.1.254
\n這個叫last usable address
\n
\n172.16.0.0/16 到 172.16.255.255/16→Class類地址
\nhost portion是2個byte = 16 bits
\n00000000 00000000
\n加1
\n00000000 00000001
\n所以十進制是.0.1
\n那172.16.0.1就是first usable address
\n
\n
\n11111111 11111111→這個是廣播地址
\n廣播地址 - 1
\n11111111 11111110
\n所以十進制是255.254
\n172.16.255.254是last usable address
\n
\nR1>en
\n
\n// 查詢router的IP介面狀況
\nR1#show ip interface brief
\n
\n// 進入global configuration mode
\nR1#conf t
\n
\nR1(config)#interface gigabitethernet 0/0
\nR1(config-if)# 
\n
\n(config-if)# 代表進入了interface configuration mode
\n
\n
\n注意一下指令
\n// gigabitethernet與0/0之間有一個空白
\nR1(config)#interface gigabitethernet 0/0
\n
\n也可以gigabitethernet與0/0之間沒有空白
\nR1(config)#interface gigabitethernet0/0
\n
\n也可以
\nR1(config)#int g0/0
\n
\n// 現在要設定router的IP地址
\nR1(config-if)# ip address 10.255.255.254 ?
\nA.B.C.D IP subnet mask 遮罩(代表network portion)
\n
\n所以指令還需要指名subnet 也就是network portion
\n這是A類地址，所以只有第一個byte是代表network portion
\nR1(config-if)# ip address 10.255.255.254 255.0.0.0
\n
\ncisco設備預設是shutdown狀態
\n現在所以要讓cisco設備no shutdown
\n再下一個指令
\nR1(config-if)#no shutdown
\n
\n// show ip interface breif
\nR1(config-if)#do sh ip int br
\n
\n設定Router IP
\nR1(config-if)# ip address 10.255.255.254 255.0.0.0
\n
\n// 可以直接ip add 比較快
\nR1(config-if)# ip add 172.16.255.254 255.255.0.0
\n
\nno shutdowm  →可以寫no shut
\n
\n// 查詢某個介面的狀態
\nR1# show interfaces g0/0
\n
\n
\n// description目前為空白
\nR1#show interfaces description
\n
\n// 為router的每個連接介面增加說明
\nR1# int g0/0
\nR1(config-if)#description ## to SW1 ##
\n
\nR1# int g0/1
\nR1(config-if)#description ## to SW2 ##
\n
\nR1# int g0/2
\nR1(config-if)#description ## to SW3 ##
\n
\n// 現在可以查看description了
\nR1(config-if)#do sh int desc