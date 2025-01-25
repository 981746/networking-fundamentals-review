A類IP地址的第一個octet 是0-127  
<br>但是因為0與127是被reserved 所以有些人會說是1-126
<br>
<br>Maximum Hosts per Network
<br>192.168.1.0/24→這是Class C地址
<br>
<br>192.168.1.0/24 到 192.168.1.255/24
<br>0到255是256種可能
<br>其實就是最後一個btye，2^8= 256種可能
<br>但是最後一個byte的decimal值是0與255會被分別保留給network與broadcast使用
<br>所以只有256 - 2 = 254種可能
<br>
<br>現在
<br>172.16.0.0/16→這是ClassB地址 到 172.16.255.255/16
<br>後面兩個byte代表的是host地址的可能數量
<br>2bytes = 16bits = 2^16 = 65,536
<br>但是host portion全是0時與全是1時，會被分別保留給network與broadcast使用
<br>也就是後面兩個byte的pattern的十進位是.0.0與.255.255會被保留
<br>65,536 - 2 = 65,534
<br>Maximum hosts per network = 2^16 -2 = 65,534
<br>
<br>再來
<br>10.0.0.0/8 →這是Class A地址
<br>後面三個byte代表的是host portion
<br>當host portion全是0時與全是1時，會被分別保留給network與broadcast使用
<br>也就是後面三個byte的pattern的十進位是.0.0.0與.255.255.255會被保留
<br>
<br>2^24 = 16,777,216
<br>Maximum hosts per network = 2^24 -2 = 16,777,214
<br>
<br>First/Last Usable Address(第一/最後 可用地址)
<br>192.168.1.0/24 到 192.168.1.255/24 →這是Class C地址
<br>所以host portion是一個byte，8個bits
<br>
<br>00000000 →增加1      0000001
<br>         十進制       1
<br>所以就是192.168.1.1
<br>這個叫first usable address
<br>
<br>
<br>11111111(廣播地址)  減1 →   11111110
<br>                    十進制   254
<br>所以就是192.168.1.254
<br>這個叫last usable address
<br>
<br>172.16.0.0/16 到 172.16.255.255/16→Class類地址
<br>host portion是2個byte = 16 bits
<br>00000000 00000000
<br>加1
<br>00000000 00000001
<br>所以十進制是.0.1
<br>那172.16.0.1就是first usable address
<br>
<br>
<br>11111111 11111111→這個是廣播地址
<br>廣播地址 - 1
<br>11111111 11111110
<br>所以十進制是255.254
<br>172.16.255.254是last usable address
<br>
<br>R1>en
<br>
<br>// 查詢router的IP介面狀況
<br>R1#show ip interface brief
<br>
<br>// 進入global configuration mode
<br>R1#conf t
<br>
<br>R1(config)#interface gigabitethernet 0/0
<br>R1(config-if)# 
<br>
<br>(config-if)# 代表進入了interface configuration mode
<br>
<br>
<br>注意一下指令
<br>// gigabitethernet與0/0之間有一個空白
<br>R1(config)#interface gigabitethernet 0/0
<br>
<br>也可以gigabitethernet與0/0之間沒有空白
<br>R1(config)#interface gigabitethernet0/0
<br>
<br>也可以
<br>R1(config)#int g0/0
<br>
<br>// 現在要設定router的IP地址
<br>R1(config-if)# ip address 10.255.255.254 ?
<br>A.B.C.D IP subnet mask 遮罩(代表network portion)
<br>
<br>所以指令還需要指名subnet 也就是network portion
<br>這是A類地址，所以只有第一個byte是代表network portion
<br>R1(config-if)# ip address 10.255.255.254 255.0.0.0
<br>
<br>cisco設備預設是shutdown狀態
<br>現在所以要讓cisco設備no shutdown
<br>再下一個指令
<br>R1(config-if)#no shutdown
<br>
<br>// show ip interface breif
<br>R1(config-if)#do sh ip int br
<br>
<br>設定Router IP
<br>R1(config-if)# ip address 10.255.255.254 255.0.0.0
<br>
<br>// 可以直接ip add 比較快
<br>R1(config-if)# ip add 172.16.255.254 255.255.0.0
<br>
<br>no shutdowm  →可以寫no shut
<br>
<br>// 查詢某個介面的狀態
<br>R1# show interfaces g0/0
<br>
<br>
<br>// description目前為空白
<br>R1#show interfaces description
<br>
<br>// 為router的每個連接介面增加說明
<br>R1# int g0/0
<br>R1(config-if)#description ## to SW1 ##
<br>
<br>R1# int g0/1
<br>R1(config-if)#description ## to SW2 ##
<br>
<br>R1# int g0/2
<br>R1(config-if)#description ## to SW3 ##
<br>
<br>// 現在可以查看description了
<br>R1(config-if)#do sh int desc