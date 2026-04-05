# RS485 test communication

Serial port parameters 9600bps 8N1 no flow control.

Connected USB dongle to the inverter and captured the example communication. The inverter is set to Li4 battery and sends repeatedly the following communication:

Modbus RTU Frame Decoded
|Byte(s)|HexValue|Description|
|--|--|--|
|1|01|1|Device Address – Slave/device ID 1|
|2|03|3|Function Code – Read Holding Registers (FC03)|
|3–4|00 13|19|Starting Register Address – Register 19 (0x0013)|
|5–6|00 11|17|Quantity of Registers – Read 17 registers|
|7–8|74 03|-|CRC16 – 0x0374 (Little-endian: low byte 74, high byte 03)|


