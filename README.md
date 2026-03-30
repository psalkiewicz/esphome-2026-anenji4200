# esphome-2026-anenji4200
Template for creating ESP Home device to replace internal datalogger module in Anenji 4200 (2026 models)

Pinout of the Wifi internal board. After disconnecting the original wifi board we have 3 pin headers with various connectivity options.
The board does wifi communications and also rises the signals to RS485/RS232 levels. The signals on the pin headers are TTL levels and can be directly connected to ESP32 or Raspberry PI.

5-pin header - RS232
|Pin|Description|
|--|--|
|1|TX (3.3V TTL level)|
|2|RX (3.3V TTL level)|
|3|+5V VCC|
|4|GND|
|5|Unknown (3.3V TTL level)|

4-pin header - RS485
|Pin|Description|
|--|--|
|1|RS485 A (3.3V TTL level)|
|2|RS485 B (3.3V TTL level)|
|3|+5V VCC|
|4|GND|

2-pin header - Unknown to be determined. One of the pins go directly into a transformer so I expect it to be some kind of AC here. Measuring with multimeter shows 0V. Maybe it requires enabling some output on the inverter.
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

RS485 RJ-45 connector
|Pin|Description|
|--|--|
|1|B|
|2|A|
|3||
|4||
|5||
|6||
|7|A|
|8|B|
