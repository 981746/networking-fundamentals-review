DAY11 Part1 LAB解答  
我PC1 ping PC2失敗的原因是，結論：
* PC1沒有去設定IP與gateway  
* PC2沒有去設定IP與gateway  
* R1有一條static route的next-hop填錯  
* 然後router並不需要去設定default route，應該是要連Internet才需要吧




//////  

PC1的gateway要填R1的ip地址  
192.168.1.254


除了PC1的gateway要設定  
啊 我也忘記設定PC1的IP地址了！

那PC2的gateway與IP地址也都要設定吧  
PC2 gateway是R3的IP：192.168.3.254  
PC2的IP是192.168.3.1


R1 PC1→PC2要通 目標網路192.168.3.0 255.255.255.0 next-hop192.168.12.0// 欸next-hop填錯了 應該是192.168.12.2

所以先拿掉原本的  
no ip route 192.168.3.0 255.255.255.0 192.168.12.0

再新增  
ip route 192.168.3.0 255.255.255.0 192.168.12.2

額~這樣算ping成功嗎 只有一次有reply  
不算！  
C:\>ping 192.168.3.1

Pinging 192.168.3.1 with 32 bytes of data:  

Request timed out.  
Request timed out.  
Request timed out.  
Reply from 192.168.3.1: bytes=32 time<1ms TTL=125

Ping statistics for 192.168.3.1:  
    Packets: Sent = 4, Received = 1, Lost = 3 (75% loss),  
Approximate round trip times in milli-seconds:  
    Minimum = 0ms, Maximum = 0ms, Average = 0ms


/////看完教學影片  
完全不需要default route  
這個好像是要連Internet才需要  
ip route 0.0.0.0 0.0.0.0 192.168.3.1  
所以拿掉no ip route 0.0.0.0 0.0.0.0 192.168.3.1

PC1再ping一次PC2，成功了！！！  
結果：  
Pinging 192.168.3.1 with 32 bytes of data:  

Reply from 192.168.3.1: bytes=32 time<1ms TTL=125  
Reply from 192.168.3.1: bytes=32 time<1ms TTL=125  
Reply from 192.168.3.1: bytes=32 time<1ms TTL=125  
Reply from 192.168.3.1: bytes=32 time<1ms TTL=125  

Ping statistics for 192.168.3.1:  
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),  
Approximate round trip times in milli-seconds:  
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
