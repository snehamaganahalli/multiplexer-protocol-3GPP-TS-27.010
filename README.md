# multiplexer-protocol-3GPP-TS-27.010

AT+CMUX=0       => Enter MUX mode with OK response
AT+CMUX=1,2,3   => Enter into MUX mode with the confiurations



        MS: Mobile Station                     TE: Terminal Equipment
           MUX/DEMUX module
    -1  [It can be Mobile]         -             [It is the PC]
    -2                    (4.single wired connection) 
    -3
(1,2,3 :DLC Channels (Data Link connection.))


**(MS<---TE)**
The data from the PC will be sent in the 3GPP frame format to the MS. The 3GPP MUX/DEMUX module will understand the frame format, it will DEMUX it to a
particular DLC.

**(TE--->MS)**
The data from the one of the DLC will be picked by the 3GPP MUX/DEMUX module, since it knows from which DLC it came, it will make a frame (MUX) and send it to the TE over the wired connection.
    
Each channel between TE and MS is called a Data Link Connection (DLC)


**Frame Format**

| Flag | Address | Control | length | Information | FCS | Flag |
1B(byte)   1B       1B        1/2 B      many B     1B    1B


Flag: specific bit pattens at both the end of the frame to identify the start and end of the frame.
Address: which DCL I (Data Link Connection/Control Identifier)
length: It will be 2B if EA bit is sent in the 1st B.
Info: Data on the frame.
FCS: Frame check sequence.
Flag: Same as above.
Control field: As below:
SABM (Set Asynchronus balanced mode): 1111 1100
UA (Unnumbered Acknowledgement): 1100 1100
DM (Disconnected Mode): 1111 1000
DISC (Disconnect): 1100 1010
UIH (Unnumbered Information with Header check): 1111 1111
UI (Unnumbered Information): 1100 1000

**SABM:**






        
