本影片專注於討論Network層的Provides logical addressing(IP addresses)

在兩個SW之間，加上一台router
這樣本來是一個網路
就被切分成了兩個網路


兩個網路
192.168.1.0/24
192.168.2.0/24

/24指的是
IP地址的哪一個部分代表network
IP地址的哪一個部分代表end host，也就是PC

the first three group of numbers represent the network
IP地址的前面三個組代表network?????

router也需要IP
need each IP of network it is connected to
G0/0 192.168.1.254
G0/1 192.168.2.254

假設從PC1送一個broadcast
資料到SW1後往PC2與R1送
但是到了R1 就不會繼續傳遞資料了

先談IPv4 header的兩個欄位
Source IP Address
Destination IP Address
都是32bit (4bytes)

192.168.1.254
(8bits).(8bits).(8bits).(8bits)

11000000.10101000.00000001.11111110→IP的binary格式

IP通常以dotted.decimal格式表示

octet我記得這是byte的意思

2進制轉10進制
10進制轉2進制

每一個octet的可能值為0~255

/24
IP地址的前面24bit(3bytes)是代表network
而剩下的1個byte就是代表end hsot

IP地址分為ABCDE類
D類 multicast address
E類  reserved address

loopback address
開頭為127的地址

127.().().() ~ 127.().().()
127.0.0.0 ~ 127.255.255.255
這些地址通常被用來測試local端設備的 network stack
舉例 去pin 127.0.0.1

ABC類各自代表network的部分是
/8 /16 /24
第一個byte
前面兩個byte
前面三個byte

class A has fewer potential network
class c and d have more network

每個網路的第一個地址是屬於網路的地址
the first address in each network is network address
不能assign給host

然後網路的最後一個address是用來廣播的
所以網路的地址要再-2


dotted decimal netmask

network portion are 1s
host portion are 0s


ClassA: /8    255.0.0.0 所以2進制是11111111.00000000.00000000.00000000
ClassB: /16   255.255.0.0          11111111.11111111.00000000.00000000
ClassC: /24   255.255.255.0        11111111.11111111.11111111.00000000

192.168.1.0/24

前面24bit (3bytes)是network
而最後1 byte是host
而當host的部分是0是，代表的是網路地址
所以192.168.1.0/24是一個網路地址
並且網路地址不能assign給host


192.168.1.255/24

前面3個byte是網路
最後一個byte是host

host的部分如果全是1，11111111→255
代表的是廣播地址
不能被assign給host
所以192.168.1.255是廣播地址