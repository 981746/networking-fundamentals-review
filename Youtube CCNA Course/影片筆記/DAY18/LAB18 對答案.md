LAB18 對答案

老師基本上都是去查running config確認目前機器上的指令

然後設定R1部分的point to point connection  
int g0/0  
ip add 10.0.0.194 255.255.255.192// 遮罩填錯了255.255.255.252


所以SW2這邊也填錯XD  
(2)設定IP  
ip routing  
int g1/0/2  
no switchport  
ip add 10.0.0.193 255.255.255.192// 遮罩填錯了是255.255.255.252


LAB18 NetSim Preview

查目前的port是access port還是trunk port？  
`interfaces f0/3 switchport`

去看PC的連線狀況，老師通常會先ping default gateway  

