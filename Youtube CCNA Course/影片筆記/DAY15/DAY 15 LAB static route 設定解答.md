DAY 15 LAB static route 設定解答

R1與兩個LAN相連接 且有一point to point connection  
R2與兩個LAN相連接 寫有一point to point connection


先在R2設定到達LAN1與LAN2的路徑：  
(1)設定R2到達LAN1的路徑  
(2)設定R2到達LAN2的路徑  
// 我那個時候在做這一題時，是以PC與另一台PC要互相相通的角度去做的，搞得很複雜  
// 我以為我對一台router做了4個route的設定  
// 現在發現有兩條是重複的，其實我跟老師這邊一樣，對一台router只做了兩個route的設定  
// 他這邊作答的邏輯很簡單  
// 在此router，這台router，要通道另外一邊的router，會通到個幾個網路，就設定幾條static route  
// 所以  
// R2要通到R1，那R1那邊接了幾個LAN，就設定幾條static route


再來R1設定到達R2後與R2相連的LAN的路徑  
(1)設定R1到達LAN3的路徑  
(2)設定R1到達LAN4的路徑