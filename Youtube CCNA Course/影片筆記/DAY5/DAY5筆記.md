一台switch 連到一個router 算一個LAN
假設switch與switch相接 他們仍算在同一個LAN

我在TCP/IP讀到的RFC894 encapsulation的frame格式是

destination地址 | source 地址 | type | data | CRC(FCS)


在CCNA Day5影片這邊
destination地址前面還多了兩個欄位欸

Preamble | SFD(Start Frame Delimiter)

RFC894 與 1042差在同一個位置是length欄位還是type欄位

那這部影片是用這一個位置的欄位的size
來判斷說 這是type欄位 還是length欄位

值比1500小，那這個欄位就是length這個類型
值是1536或比1536大，那這個欄位就是type類型，這個來欄位用來表示使用的是IPv4還是IPv6
所以如果值是
0x0800(16進制) 也就是(2048十進制) →代表使用的是IPv4
0x86DD               (34525)      →代表使用的是IPv6
 
MAC Address或稱BIA(Burned-In-Address)
48bit(6個byte)
globally unique
也有一種類型是locally unique address

前面3個bytes是OUI(organizationally unique identifier)
後面3個bytes是用來識別裝置 而且是unique的
MAC地址以12個16進制字元來表示


16進制的1D

16 ^ 0 * D + 16^1 *1 = 13 + 16 = 29

這邊的MAC的AAAA 代表的OUI 也就是組織的識別

從PC1 把資料送到 PC2
這樣的frame叫做Unicast frame
這個frame只有一個目標 一個目的地 也就是PC2

PC1 → switch → PC2
因為pc1要到pc2，一定要先經過switch
switch接受到pc1的資料後 會產生一個MAC Address Table
這個table會存 MAC地址(.0001)、所使用的介面(F0/1) 相關資訊

產生MAC Address Table這樣的活動叫Dynamically learnd MAC address
Dynamic MAC address

因為我們不是手動把MAC地址輸入進switch
而是switch自己學習到有新的硬體地址

但這個例子現在有一個問題
switch從PC1收到frame
frame會註明說要把資料送到PC2 填寫的是PC2的MAC地址
可是switch並不知道 PC2的MAC地址 這台設備在哪啊？
這種情況就要Unknown Unicast frame

因此switch必須要有FLOOD的動作 
同時把資料傳送到連到的這台switch的PC(扣掉來源的那台PC)
資料傳到PC3 此時switch就有了PC3的MAC 就知道說不是這台 所以把packet丟掉
資料傳到PC2 此時switch就有了PC2的MAC 知道是這台 就照正常的OSI Stack去處理資料
此時 除非PC2有要傳遞資料給其他台電腦 也就是說會經過switch
這樣switch才可能會有PC2的MAC地址在 MAC Address Table

所以經過了FLOOD
Switch還是沒有PC2的MAC地址

unknown unicast frame flooded
known unicast frame forwarded

Dynamic MAC Address
如果是cisco設備的話 五分鐘沒有被使用到 就會被移除


再一個例子
PC1要傳東西給PC3
兩台PC之間有兩台switch

PC1先到SW1 
發生flooding
所以SW1 flood給pc2 與sw2
pc2 拋棄packet

sw2收到frame
此時sw2的MAC Address Table
收到來源MAC地址 與連接的介面
注意！這面這邊填F0/3，因為上一個node是switch1 而不是PC2


這個例子是 PC3再傳資料回去PC1
注意MAC Address Table的填寫