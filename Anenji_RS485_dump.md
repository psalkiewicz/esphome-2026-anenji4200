# RS485 test communication

Took the RS485/WiFi board and desoldered the CPU which is EB-WFBLE-03. Module works without the CPU - the RS485 communication with the inverter is active. The role of microcontroller was a wifi enabled datalogger. In the process of reverse engineering the device datalogger was replaced with ESP32 which is the purpose of this repo.

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


