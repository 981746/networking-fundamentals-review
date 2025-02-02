先自己作答看看

1. PC1 pings PC4.  
Identify the src/dst MAC address at each specified point in the route to PC4.  
Identify the MAC address by the device and interface (ie. the MAC of R1 G0/0)
Use the CLI and Packet Tracer's simulation mode to verify your answers.  
(Before you enter simulation mode, ping once to complete ARP/the MAC learning process.)
    ```
    A. Source/Destination MAC at PC1 → SW1 segment  
    Src MAC 00D0.BA11.1111(PC1 MAC)  
    Dst 0000.01AA.AAAA(R1 G0/0MAC)


    B. Source/Destination MAC at SW1 → R1 segment  
    // 在Switch這個節點，Src與Dst MAC不會改動  
    Src MAC 00D0.BA11.1111(PC1 MAC)  
    Dst 0000.01AA.AAAA(R1 G0/0 MAC)


    C. Source/Destination MAC at R1 → R2 segment  
    Src MAC 0000.01BB.BBBB(R1 G0/1)  
    Dst MAC 0000.01CC.CCCC(R2 G0/0)


    D. Source/Destination MAC at R2 → R3 segment  
    Src MAC 0000.01DD.DDDD(R2 G0/1)  
    Dst MAC 0000.01EE.EEEE(R3 G0/0)


    E. Source/Destination MAC at R3 → SW2 segment  
    Src MAC 0000.01FF.FFFF(R3 G0/1)  
    Dst MAC 000C.8544.4444(PC4 MAC)



    F. Source/Destination MAC at SW2 → PC4 segment  
    Src MAC 0000.01FF.FFFF(R3 G0/1)  
    Dst MAC 000C.8544.4444(PC4 MAC)



    ```


2. PC1 pings PC3.  
Identify the src/dst MAC address at each specified point in the route to PC3.  
Identify the MAC address by the device and interface (ie. the MAC of R1 G0/0)  
Use the CLI and Packet Tracer's simulation mode to verify your answers.  
(Before you enter simulation mode, ping once to complete ARP/the MAC learning process.)

    ```
    A. Source/Destination MAC at PC1 → SW1  
    Src MAC 00D0.BA11.1111(PC1 MAC)  
    Dst MAC 0010.1133.3333(PC3 MAC)

    B. Source/Destination MAC at SW1 → PC3  
    Src MAC 00D0.BA11.1111(PC1 MAC)  
    Dst MAC 0010.1133.3333(PC3 MAC)
    ```



3. PC4 pings PC1.  
Identify the src/dst MAC address at each specified point in the route to PC1.  
Identify the MAC address by the device and interface (ie. the MAC of R1 G0/0).

    ```
    *PC4→SW2  
    Src MAC 000C.8544.4444(PC4 MAC)  
    Dst MAC 0000.01FF.FFFF(R3 G0/1)

    *SW2→R3  
    Src MAC 000C.8544.4444(PC4 MAC)  
    Dst MAC 0000.01FF.FFFF(R3 G0/1)

    *R3→R2  
    Src MAC 0000.01EE.EEEE(R3 G0/0)  
    Dst MAC 0000.01DD.DDDD(R2 G0/1)


    *R2→R1  
    Src MAC 0000.01CC.CCCC(R2 G0/0)  
    Dst MAC 0000.01BB.BBBB(R1 G0/1)

    *R1→SW1  
    Src MAC 0000.01AA.AAAA(R1 G0/0)  
    Dst MAC 00D0.BA11.1111(PC1 MAC)


    *SW1→PC1  
    Src MAC 0000.01AA.AAAA(R1 G0/0)  
    Dst MAC 00D0.BA11.1111(PC1 MAC)
    ```



WRITE YOUR ANSWERS IN THE COMMENT SECTION OF THE VIDEO :)