A類IP地址的第一個octet 是0-127
但是因為0與127是被reserved 所以有些人會說是1-126

Maximum Hosts per Network
192.168.1.0/24→這是Class C地址

192.168.1.0/24 到 192.168.1.255/24
0到255是256種可能
其實就是最後一個btye，2^8= 256種可能
但是最後一個byte的decimal值是0與255會被分別保留給network與broadcast使用
所以只有256 - 2 = 254種可能

現在
172.16.0.0/16→這是ClassB地址 到 172.16.255.255/16
後面兩個byte代表的是host地址的可能數量
2bytes = 16bits = 2^16 = 65,536
但是host portion全是0時與全是1時，會被分別保留給network與broadcast使用
也就是後面兩個byte的pattern的十進位是.0.0與.255.255會被保留
65,536 - 2 = 65,534
Maximum hosts per network = 2^16 -2 = 65,534

再來
10.0.0.0/8 →這是Class A地址
後面三個byte代表的是host portion
當host portion全是0時與全是1時，會被分別保留給network與broadcast使用
也就是後面三個byte的pattern的十進位是.0.0.0與.255.255.255會被保留

2^24 = 16,777,216
Maximum hosts per network = 2^24 -2 = 16,777,214

First/Last Usable Address(第一/最後 可用地址)
192.168.1.0/24 到 192.168.1.255/24 →這是Class C地址
所以host portion是一個byte，8個bits

00000000 →增加1      0000001
         十進制       1
所以就是192.168.1.1
這個叫first usable address


11111111(廣播地址)  減1 →   11111110
                    十進制   254
所以就是192.168.1.254
這個叫last usable address

172.16.0.0/16 到 172.16.255.255/16→Class類地址
host portion是2個byte = 16 bits
00000000 00000000
加1
00000000 00000001
所以十進制是.0.1
那172.16.0.1就是first usable address


11111111 11111111→這個是廣播地址
廣播地址 - 1
11111111 11111110
所以十進制是255.254
172.16.255.254是last usable address

R1>en

// 查詢router的IP介面狀況
R1#show ip interface brief

// 進入global configuration mode
R1#conf t

R1(config)#interface gigabitethernet 0/0
R1(config-if)# 

(config-if)# 代表進入了interface configuration mode


注意一下指令
// gigabitethernet與0/0之間有一個空白
R1(config)#interface gigabitethernet 0/0

也可以gigabitethernet與0/0之間沒有空白
R1(config)#interface gigabitethernet0/0

也可以
R1(config)#int g0/0

// 現在要設定router的IP地址
R1(config-if)# ip address 10.255.255.254 ?
A.B.C.D IP subnet mask 遮罩(代表network portion)

所以指令還需要指名subnet 也就是network portion
這是A類地址，所以只有第一個byte是代表network portion
R1(config-if)# ip address 10.255.255.254 255.0.0.0

cisco設備預設是shutdown狀態
現在所以要讓cisco設備no shutdown
再下一個指令
R1(config-if)#no shutdown

// show ip interface breif
R1(config-if)#do sh ip int br

設定Router IP
R1(config-if)# ip address 10.255.255.254 255.0.0.0

// 可以直接ip add 比較快
R1(config-if)# ip add 172.16.255.254 255.255.0.0

no shutdowm  →可以寫no shut

// 查詢某個介面的狀態
R1# show interfaces g0/0


// description目前為空白
R1#show interfaces description

// 為router的每個連接介面增加說明
R1# int g0/0
R1(config-if)#description ## to SW1 ##

R1# int g0/1
R1(config-if)#description ## to SW2 ##

R1# int g0/2
R1(config-if)#description ## to SW3 ##

// 現在可以查看description了
R1(config-if)#do sh int desc
> Written with [StackEdit](https://stackedit.io/).