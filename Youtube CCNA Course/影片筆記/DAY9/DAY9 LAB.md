1. Configure the hostname of R1, SW1, and SW2

2. Configure the appropriate IP addresses on R1, PC1, PC2, PC3, PC4

    // 前面兩個問題很簡單跳過

3. Manually configure the speed and duplex on interfaces connected to other 
    networking devices (not end hosts)

4. Configure appropriate descriptions on each interface

5. Disable interfaces which are not connected to other devices

	    A：
	    
	    R1 互相連接 SW1
	    
	    *在R1的系統
	    老師手動去改了Speed與duplex
	    speed 1000
	    duplex full
	    因為是router最後要記得設定no shutdown
	    還要記得存檔下write指令
	    // 但是在packet tracer比較奇怪的是
	    // 改完了設定，去查看sh ip int br
	    // protocol欄位是down，所以packet tracer有問題
	    
	    *在SW1的系統
	    剛剛在R1手動改了speed與duplex
	    所以現在在SW1查sh int status
	    與R1連接的介面Gig0/1  此時的Status欄位 顯示notconnect
	    
	    而SW1與SW2，兩台之間互相連接，尚未手動去修改speed and duplex
	    所以此時
	    SW1與SW2連接的介面Gig0/2的Status蘭欸，顯示為connected
	    
	    那我們現在去改SW1與R1連接的介面Gig0/1的speed and duplex
	    SW1(config)#int g0/1
	    SW1(config-if)#speed 1000
	    SW1(config-if)#duplex full
	    
	    
	    SW1也有與SW2連，所以也要去改Gig0/2的Speed and duplex
	    SW1(config)#int g0/2
	    SW1(config-if)#speed 1000
	    SW1(config-if)#duplex full
	    
	    題目說與host連接(PC)不用去設定spped and duplex
	    所以SW1就設定g0/1 and g0/2就好，不用去設定f0/1 and f0/2
	    
	    
	    那SW1的f0/3 - 24界面都沒有在使用，沒有連接任何PC
	    必須去修改description為not in use
	    更重要的是必須把f0/3 - 24全部都shutdown
	    指令為
	    int range f0/3 - 24
	    shutdown
	    都shutdown之後，查sh int status，f0/3 - 24的status欄位會顯示disabled
	    此時與SW2連接的G0/2的status欄位會顯示notconnect
	    因為我們從SW1這邊手動修改了speed and duplex
	    但是SW2那邊還沒有做對應的修改
	    
	    查sh int status還有一個地方很奇怪
	    手動修改了SW1的g0/1 and g0/2的speed and duplex介面
	    但是相關欄位顯示a-full, a-1000010/100BaseTX
	    這是不正確的顯示，packet tracer有問題
	    
	    最後記得幫SW1存檔，下write指令
	    
	    
	    *在SW2的系統
	    在SW2只要改g0/1介面的speed and duplex就好(因為SW1只有與SW2設備去連接)
	    並且把其他沒有用到的介面關掉：使用int range指令
	    最後記得存檔，使用write指令

> Written with [StackEdit](https://stackedit.io/).
