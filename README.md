# esphome-2026-anenji4300
Template for creating ESP Home device to replace internal datalogger module in Anenji 4300 (2026 models)

Pinout of the Wifi internal board. After disconnecting the original wifi board we have 3 pin headers with various connectivity options.
The board does wifi communications and also rises the signals to RS485/RS232 levels. The signals on the pin headers are TTL levels and can be directly connected to ESP32 or Raspberry PI.

5-pin header
|Pin|Description|
|--|--|
|1|TX (3.3V TTL level)|
|2|RX (3.3V TTL level)|
|3|+5V VCC|
|4|GND|
|5|Unknown (3.3V TTL level)|

4-pin header
|Pin|Description|
|--|--|
|1|RS485 A (3.3V TTL level)|
|2|RS485 B (3.3V TTL level)|
|3|+5V VCC|
|4|GND|

2-pin header
|Pin|Description|
|--|--|
|1|Unknown|
|2|Unknown|

Please connect the 4 cables from 5-pin header to ESP32:

|Pin on ESP32|Pin on 5-pin header|
|--|--|
|GPIO16 RXD | 1 - TX |
|GPIO17 TXD | 2 - RX |
|VIN        | 3 - +5VCC |
|GND        | 4 - GND |
