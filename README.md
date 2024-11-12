# multiplexer-protocol-3GPP-TS-27.010

AT+CMUX=0         => Enter MUX mode with OK response. 0 represents to the Basic MUX mode. And the control channel will be opened (DLCI 0)
AT+CMUX=0,1,2,3   => Enter into MUX mode with the confiurations



        MS: Mobile Station                     TE: Terminal Equipment
           MUX/DEMUX module
    -0  [It can be Mobile]         -             [It is the PC]
    -1                    (4.single wired connection) 
    -2
    -3
(0,1,2,3 :DLC Channels (Data Link connection.)) One of them is the control channel.


**(MS<---TE)**
The data from the PC will be sent in the 3GPP frame format to the MS. The 3GPP MUX/DEMUX module will understand the frame format, it will DEMUX it to a
particular DLC.

**(TE--->MS)**
The data from the one of the DLC will be picked by the 3GPP MUX/DEMUX module, since it knows from which DLC it came, it will make a frame (MUX) and send it to the TE over the wired connection.
    
Each channel between TE and MS is called a Data Link Connection (DLC)


**Frame Format**

| Flag | Address | Control | length | Information | FCS | Flag |

| 1B(byte) | 1B | 1B | 1/2 B | many B | 1B | 1B


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
UIH (Unnumbered Information with Header check): 1111 1111, FCS is calculated only over the address and control field
UI (Unnumbered Information): 1100 1000, FCS is calculated over all the fields.

**SABM/DISC:**
MS <--SABM/DISC-- TE
MS ---UA-->  TE

Whenever the SABM command is sent, the command has which DLC should be opened. The particular DLC will be opened. 
Send it on DLC0. Reply is given with UA.

DISC command is sent to close the DLC opened by the SABM command

**UI/UIH**:
Data is transferred through UIH/UI.

**DM:**
Close all the DLCs. The setup is already in disconnected mode send OK.
This is same as CLD.

**CLD:**
Close all the DLC and close the MUX mode. To restart MUX mode send AT+CMUX=0

======================================
You can put MS to sleep/low power stated so that power is saved.

Suppose you are using a UART as the serial port, then you can OFF the UART clock by calling IOCTL.

ioctl(uart_fd, CLOCK_OFF)

To make UART wakeup

ioctl(uart_fd, CLOCK_ON)

======================================
Break detection on UART:
When we unplug the cable, the "/dev/tty" gets closed. Before that we receive a continuous sequence of 1s on the UART.

======================================

You can send the new configurations for AT+CMUX setup. AT+CMUX=0 means default config is applied.
This can be done by Parameter Negotiation command.

In "Information field" of the frame, we can have TLV (Type, length, Value1, Value2, Value3 .... valuen)
Eg:                                                                    T1    T2       which DLC etc...


For PSC, Type is 0000 0010, length is 0, no value filed.

===================================================

**T2:**
The T2 timer is the amount of time the multiplexer control channel waits before re-transmitting a command.


        
