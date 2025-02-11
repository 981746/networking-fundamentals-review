DAY17 LAB 解答
// 解答完LAB後，還幫我們preview了NetSim，NetSim裡有很多LAB可以做

網路圖用文字說明一下：  
SW1 == VLAN10 PC1 PC2  
SW1 == VLAN30 PC3 PC4

SW2 == VAN20 PC5  
SW2 == VAN10 PC7 PC8  
SW2 == R1



看過解答後，我覺得重點是：

1.Switch與PC用access port模式連接，並且註明VLAN為何  
所以Switch與PC有直接連接的話，這台Switch也就有相關的VLAN紀錄

2.SW1與SW2用trunk port模式連接  
在這種狀況下SW1與SW2都必須去做trunk port設定

但是在第二題發生一個狀況SW2下sh int trunk指令  
顯示訊息：Vlans allowed and active in management domain  
我們想要允許VLAN10,30通過SW1與SW2的trunk port  
可是active的VLAN只有10 還少了一個30  
原因是因為SW1直接有access port與VLAN10、VLAN30裡的PC相連  
而SW2的access port只有與VLAN10的PC相連  
所以SW2的g0/1介面當然沒有VLAN30的紀錄

所以直接在SW2的g0/1介面，下指令vlan 30  
這樣就可以了

3.Router與Switch，一端以Router on a stick方式、一端以trunk port方式相連時  
Router端要先讓介面no shutdown，再去切子介面與設定IP(gateway最好用last usable address)  
Switch端要去設定trunk port，並且允許相關的VLAN