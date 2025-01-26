Cisco IOS CLI 思科系統的CLI介面  
也有GUI

console port可以連接CLI，也可以遠端登入CLI  
兩種類型的console port  
USB mini B  
RJ45

rollover cable用來接console port ：一端是RJ45，另一端好像是DVI接頭

現在的筆電通常沒辦法用  
所以需要轉接線

rollover cable的資料傳輸有8個pin  
訊號的傳送的多條路會在中心匯聚成一點

rollover 接上後，要用putty軟體以serial選項登入CLI  
// putty我好像用過欸

serial選項初始設定值：  
speed 9600  
data bits:8  
stop bit: 1  
each 8 of data, one stop bit is sent to mock the end of the bits  
parity:none// use to detect error  
flow control:none

第一次登入CLI會提示要不要進入initial dialog  
他回答no  
然後按enter

此時預設會進入user exec mode  
Router>  
router後面的大於符號就代表進入user exec mode

Router>的Router代表的是hostname of the device

user exec mode也稱user mode，基本上在此模式並不能做甚麼修改  功能上非常受限

下enable指令，就可以進入Privileged EXEC Mode，代表符號#  
Router>enable  
Router#

在Privileged EXEC Mode  
可以查看設定、重啟設備 等等等  
此模式並不能修改設定 但可以改時間 儲存設定檔



下達?指令，查看可使用的指令  
Router>?  
or  
Router#?

輸入en ，再使用tab 命令補全  
這樣就會自動補出enable這個指令

額 甚至輸入en  
就可以直接進入privilege模式

輸入e?  
會把所有與e相關的指令列出  
Router>e?  
enable exit

輸入configure terminal字詞進入global configuration mode  
進入此模式後，會有一個括號config的字樣  
Router(config)#

然後有一個更快速的方法  
輸入conf t進入global configuration mode

必須在global configuration mode才能改密碼  
enable password CCNA  
// 把密碼改成CCNA(要注意大小寫)

輸入exit指令離開global configuration mode模式

兩種設定檔  
running-config:機器目前正在跑的設定檔  
startup-config：機器重啟會執行的設定檔

Router# show running-config

出現內容：  
....  
...  
enable password CCNA//我們剛剛的設定

Router# show startup-config  
startup-config is not present  
那是因為我們還沒儲存目前的設定檔  
所以每次重開機後，會載入預設的設定檔而不是startup-config  
那現在我們來存檔

儲存設定檔的三種方法  
Router#write  
Router#write memory  
Router#copy running-config startup-config  
// 把running-config複製到startup-config

目前的密碼是明碼存在系統中  
所以要加密

加密密碼指令service password-encryption

Router(config)#service password-encryption

type 7的密碼加密類型還是不OK  
網路上幾秒就能破解

目前的密碼是明碼存在系統中  
所以要加密

加密密碼指令service password-encryption

Router(config)#enable secret Cisco

Router(config)#do sh run// 使用do指令 在global模式下查看running config

5→MD5

如果設定檔的enable password與enable secret同時存在  
enable password會被忽略

取消設定檔中的指令效果 使用no  
Router(config)#no service password-encryption  
Router(config)#do sh run

//這樣子代表  
目前的password被加密的效果仍在  
只是如果有修改新的password，新的密碼不會被加密  
而password與secret指令無關

下加密指令：  
現在的密碼會被加密、未來的密碼也會被加密  
password與secret指令無關

看這部影片大概花了一小時