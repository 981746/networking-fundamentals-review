#### 有疑問的部分：
```
PC 互相連接 Switch
所以是Switch還是PC的autonegotiation關掉的狀況？
// 有點看不懂這段的英文敘述
// 是對方，也就是PC端關掉autonegotiation嗎？因為對方關掉就無法正常溝通，所以switch會自動去sense嗎

Speed
switch會去自動感知另外一段設備的速度
如果感知失敗，就會使用支援速度中的最慢的速度
舉例： 一個介面支援10/100/1000Mbs，此時會使用10Mbs

Duplex
if 抓到的Speed是10Mbs 或 100Mbs，此時會使用half-duplex
if 抓到的Speed是1000Mbs或更快的速度，此時會用full-duplex
```

#### 隨手筆記：

ASR 1000-X Router  
Catalyst 9200 Switch

SW1>en  
SW1#sh ip int br

Statuc(Layer1) Protocol(Layer2)  
up             up


switch的預設狀態就是up and up

IP-Address 會維持unassigned  
因為這是layer2 switch ports


switch上還有其他的interface未連接上任何設備 所以狀態是down down  
down down(unconnected to another device)與administratively down and down是不同的(shutdown)  
SW1#show interfaces status

Name field為空白  
Name field is the description fo the interface  

SW1#conf t  
SW1(config)#int f0/1  
SW1(config-if)#speed ?  
SW1(config-if)#speed 100  
SW1(config-if)#duplex ?  
SW1(config-if)#duplex full  
SW1(config-if)#description ## to R1 ##  
SW1#sh int status

注意這三個欄位的值變成
```
Name             Duplex    Speed
## to R1 ##      full      100
```

而本來是
```
Name             Duplex    Speed
                 a-full    a-100
```
a-的a代表auto

一般來說，我們會讓switch的auto模式為打開的狀態  
但是如果有特殊狀況，你也要會手動去設定


一次設定多個介面的description，並把設備全都shutdown  
SW1(config)#interface range f0/5 - 12  
SW1(config-if-range)#description ## not in use ##  
SW1(config-if-range)#shutdown

也可以挑5到6號 交集 9到12號這樣的方式去設定多個介面  
SW1(config)#interface range f0/5 - 6, f0/9 -12  
SW1(config)#no shutdown

half duplex  
full duplex

where does half-duplex in use? In moden days, almost nowhere

old type network device  
a Hub is much simple than swith  
simply a repeated

data從兩個方向經過Hub，結果發生碰撞

CSMA/CD用來處理collision的問題

hub layer1  
switch 在layer2 並且有更多的功能

Runts
Giants
CRC
Frame
Input errors
Output errors

Autonegotiation



