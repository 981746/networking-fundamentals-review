DAY10  
這一課關注的是Layer 3 Header

IPv5 internet stream protocol  
實驗性質的protocol


Version欄位會記錄使用哪一種IP版本  
```
Length: 4bits  
IPv4 = 4(0100)  
IPv6 = 6(0110)

IPv4與IPv6的Header Structure不同  
如果Version欄位紀錄的值表示這個Header是IPv6版本  
那麼接下的欄位會與IPv4不同
```

IHL(Internet Header Length)  
```
Length: 4 bits// a half of octet

*The final field of the IPv4 header(Options) is variable in length  
so this field is necessary to indicate the total length of the header

*Indentifies the length of the header in 4-byte increments  
以4-bytes increments的方式去紀錄header length  
如果IHL欄位紀錄的值是5，那5 * 4 = 20 bytes  
就表示length of header就是20 bytes

*IHL欄位的最小值為5(=20 bytes)  
IP packet with out any IP option at the end  
代表empty option field

*IHL欄位的最大值是15(=15*4 = 60 bytes)  
如果4 bit是1111，那decimal數值就是15  
那15再去乘以4，就等於60

等於IP Option欄位是40 bytes  
60 - 20(empty option狀況) = 40


*Minimum IPv4 Header Length = 20 bytes  
*Maximum IPv4 Header Length = 60 bytes
```

DSCP欄位  
```
DSCP(Differentiated Services Code Point)  
length: 6 bits

*Used for QoS(Quality of Service)  
*Used to prioritize delay-sensitive data(streaming voice, video,etc)  
很多資料在傳輸，會依照狀況給不同的優先序
```

ECN欄位  
```
ECN(Explicit Congestion Notification)  
length: 2bits  
當網路擁塞時，提供end-to-end通知，並且不丟棄packet

通常在網路擁塞時，packet會被丟棄  
所以我們可以在ECN欄位去註明，這個資料在網路擁塞時要通知且不能丟棄  
但是ECN功能必須both endpoints都有相關的網路基礎設施去支援
```

Total Length欄位  
```
length: 16bits// 2 octet

*表明這個packet的total length為何(L3 Header + L4 Segment)  
*單位是bytes  
*最小值是20(IPv4 Header + no encapsulated data)  
*最大值是65536(16 bit的最大值)
```

Identification欄位  
```
lenght: 16bits

*如果一個packet太大了，所以被切分(fragmented)  
那這個欄位會去識別這個fragment屬於哪一個packet

*同樣的packet的fragments 他們的IPv4 Header裡的Identification欄位值都是一樣的值  
就可以用這樣的方式去識別  
然後重新組裝起來

*如果packet大於MTU(Maximum Transmission Units)  
那此packet就會被fragmented 被切分

*MTU一般是1500bytes  
記得一個Ethernet Frame size的最大值是多少嗎？1500 bytes

*fragments會在receiving host端重新被組裝起來
```

Flag欄位  
```
length: 3bits

*用來控制/識別 fragments  
*Bit0: 此bit被保留，永遠被設定成0  
*Bit1:叫做dont't fragment(DF bit)  
如果被設定成1  
那就表示這個packet不應該被fragmented

*Bit2:More Fragments(MF bit)  
如果被設成1  
代表這個packet有多個fragments  
packet的最後一個fragment，這個欄位會被設定成0

*如果這個packet是unfragmented packet  
那MF值也是設定成0
```

Fragment Offset欄位  
```
length: 13

一個packet被分為多個fragments  
這個欄位會記錄此fragment位於「原始未被拆分的packet」的哪一個位置  
這樣後續能以此順序來組裝fragments  
```

Time To Live欄位  
```
length: 8 bits

* TTL為0的話，router會丟棄這個packet  
* 所以這個欄位會防止無窮迴圈  
* 本來此欄位設於是用來表示packet的最大生命週期in seconds  
* 但實際，此欄位表達的是hop count,每一次packet到達router  
router會把TTL的值減1  
直到packet到達目的地或是TTL變成0丟棄packet  
*目前建議的TTL的最大值為64
```

Protocol欄位  
```
length: 8 bits  
*表示 L4PDU用的是哪一個protocol？  
*值為6，表示用的是TCP  
*值為17，表示用的是UDP  
*值為1，表示ICMP  
*值為89，表示OSPF(open shortest path first)  
dynamic routing protocol
```

Header Checksum欄位  
```
length: 16 bits  
*a calculated checksum

*當router接收到一個packet，router會計算checksum  
並且與此欄位紀錄的calculated checksum值去做比對  

*如果比對結果顯示不一致，那就表示傳輸的過程發生錯誤  
router就會把packet丟掉

*此欄位是去確認IPv4 Header在傳輸過程中是否有發生錯誤，跟data無關

*encapsulated data是由專責的protocol去負責確認是否有錯誤  
TCP與UDP就是負責這件事
```


Source/Destination IP Address欄位  
```
length:32 bits(IPv4地址是32bits)

*Source IP Address代表的是，誰發送這個packet  
*Destination IP Address代表的是，誰接受這個packet

Options欄位  
length:0-320bits(最大40bytes)

*這個欄位很少使用  
*如果IHL紀錄的值大於5，那就代表有使用Options欄位
```


#### 進入wireshark部分  
ethernet header of the frame is highlighted

R1#ping 192.168.1.2 size 10000  
會發送10000 bytes ping  
比MTU size 1500 bytes大  
所以這樣會導致fragmentation的狀況


Reassambled in #13  
no.13 is a ICMP request 


這一次R1#ping 192.168.1.2 df-bit// don't fragment bit  
沒有設size  
所以會用ping預設的100 bytes

那如果有設定df-bit 又傳了一個很大size的packet呢？  
這一次R1#ping 192.168.1.2 size 10000 df-bit  
// 這樣ping會failed  
Success rate is 0 percent(0/5)