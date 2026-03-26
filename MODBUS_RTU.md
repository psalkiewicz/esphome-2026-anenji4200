# MODBUS_RTU Communication

## Communication data format

During communication, the data is sent back in the form of word (WORD — 2 bytes). In each word sent back, the high byte is in the front and the low byte is in the back.If two words are sent back consecutively (such as long shaping), the high word is in the front and the low word is in the back.

|Data type|Number of registers|Number of bytes|Explain|
|--|--|--|--|
|Character type|1|1|Send back two characters at a time, and use 0 to supplement less than two characters.|
|Integer|1|2|Send back once, high byte first, low byte last|
|Long|2|4|Send it back in two words, with the high word in the front and the low word in the back.|

## Frame format

### Register content query (function code 03H)

The start address and end address of the query must be the start address and end address of a complete data block,Otherwise, the data returned is incorrect.For example, if the start address of the register of the device serial number is 186 and the length is 12, the start address cannot be between 186 and 198 during the query.Similarly, the end address (start address + number of registers read) cannot fall within this range.

Frame format sent by upper computer:

| Byte Order | Code | Examples | Explain |
|---|---|---|---|
| 0 | Device address | 01H | Device address (1 ~ 247) |
| 1 | 03H | 03H | Function Code |
| 2 | Start Register Address High Byte | 00H | Upper 8 bits of register address |
| 3 | Start register address low byte | 10H | Lower 8 bits of register address |
| 4 | Number of registers high byte | 00H | Upper 8 bits of register number |
| 5 | Low byte of register number | 02H | Number of registers Lower 8 bits |
| 6 | CRC16 check high byte | C0H | High 8 bits of CRC16 check |
| 7 | CRC16 check low byte | CBH | CRC16 check lower 8 bits |

The lower computer parses the returned frame format successfully:

| Byte Order | Code | Explain |
|---|---|---|
| 0 | Device address | Device address (1 ~ 247) |
| 1 | 03H | Function Code |
| 2 | Number of bytes of data sent back | N = number of registers requested * 2 |
| 3 | 1st register data high byte | |
| 4 | 1st register data low byte | |
| ... | ... | ... |
| ... | ... | ... |
|  | Nth register data high byte | |
|  | Nth register data low byte | |
| N+3 | CRC16 check high byte | The bytes before the CRC are checked |
| N+4 | CRC16 check low byte | |

Lower computer analysis error return frame format:

| Byte Order | Code | Explain |
|---|---|---|
| 0 | Device address | Device address (1 ~ 247) |
| 1 | 03H | Function Code |
| 2 | Number of bytes of data sent back | N = number of registers requested * 2 |
| 3 | 1st 0 | A total of N zeros are looped back |
| 4 | 2nd 0 | |
| ... | ... | ... |
| ... | ... | ... |
|  | Nth-1st 0 | |
|  | Nth 0 | |
| N+3 | CRC16 check high byte | The bytes before the CRC are checked |
| N+4 | CRC16 check low byte | |

Register reading example:

Read the data from the RMS value of the mains voltage (start register 202) to the average value of the mains power, where the mains voltage returns 220.0 V,Mains frequency returns 50.0 Hz and average mains power returns 1200 w
Host computer: `01 03 00 CA 00 03 25 F5`
Lower computer: `01 03 06 08 FC 13 88 04 B0 F7 F3`

### Register content setting (function code 10 H)

Frame format sent by upper computer:

| Byte Order | Code | Examples | Explain |
|---|---|---|---|
| 0 | Device address | 01H | Device address (1 ~ 247) |
| 1 | 10H | 10H | Function Code |
| 2 | Start Register Address High Byte | 01H | Upper 8 bits of register address |
| 3 | Start register address low byte | 10H | Lower 8 bits of register address |
| 4 | Number of registers high byte | 00H | Upper 8 bits of the number of registers (constantly equal to 0) |
| 5 | Low byte of register number | 02H | Number of registers Lower 8 bits |
| 6 | Number of bytes to be written | | N = Number of registers * 2 |
| 7 | 1st register data high byte | | |
| 8 | 1st register data low byte | | |
| ... | ... | ... | ... |
| ... | ... | ... | ... |
|  | Nth register data high byte | | |
|  | Nth register data low byte | | |
| N+7 | CRC16 check high byte | | High 8 bits of CRC16 check |
| N+8| CRC16 check low byte | | CRC16 check lower 8 bits |

The lower computer parses the returned frame format successfully:

| Byte Order | Code | Examples | Explain |
|---|---|---|---|
| 0 | Device address | 01H | Device address (1 ~ 247) |
| 1 | 10H | 10H | Function Code |
| 2 | Start Register Address High Byte | 01H | Upper 8 bits of register address |
| 3 | Start register address low byte | 10H | Lower 8 bits of register address |
| 4 | Number of registers high byte | 00H | Upper 8 bits of the number of registers (constantly equal to 0) |
| 5 | Low byte of register number | 02H | Number of registers Lower 8 bits |
| 6 | CRC16 check high byte | 41H | High 8 bits of CRC16 check |
| 7 | CRC16 check low byte | F1H | CRC16 check lower 8 bits |

Lower computer analysis error return frame format:

| Byte Order | Code | Explain |
|---|---|---|
| 0 | Device address | Device address (1 ~ 247) |
| 1 | 90H | Function Code |
| 2 | Error code | Error code |
| 3 | CRC16 check high byte | High 8 bits of CRC16 check |
| 4 | CRC16 check low byte | CRC16 check lower 8 bits |

Error code description:

| Code | Explain |
|---|---|
| 01H | Read-only register operated on |
| 03H | Write data that exceeds the acceptable range |
| 07H | Registers that are not allowed to be modified in the current operating mode |

Register write example:
Set the output voltage (start register 320) to 220V
Host computer: `01 10 01 40 00 01 02 08 98 BE 3A`
Lower computer: `01 10 01 40 00 01 01 E1`

Device register address:
- R: Indicates that the 03 H command is supported if it is readable. W: means writable, that is, supports 10 H commands. Int: Integer; Long: 
- Long;UInt: unsigned integer; ULong: unsigned long integer; ASC: ASCII code Max: maximum value; Min: minimum value
- Addresses in the following table are in decimal notation

| Data name | Unit | Data format | Start address | Number of registers | Read and write | Remark |
|---|---|---|---|---|---|---|
| Equipment fault code | | ULong | 100 | 2 | R | 32-bit fault code. Each bit corresponds to a fault code. See the fault code table for details. Fault code 1 corresponds to bit1, fault code 2 corresponds to bit2, and so on. |
| Reserved | | | 102 | 2 | | Reserved address |
| Obtain the warning code for unmasked processing | | | 104 | 2 | | The 32-bit warning code is described in the warning code description |
| Reserved | | | 106 | 2 | | Reserved address |
| Obtain the warning code after shield processing | | ULong | 108 | 2 | R/W | The 32-bit warning code is described in the warning code description |
| Reserved | | | 110 | 61 | | Reserved address |
| Device type | | UInt | 171 | 1 | R | |
| Device name | | ASC | 172 | 12 | R/W | Device name, written or read in ASCII |
| Invalid data | | UInt | 184 | 1 | R | Agreement number, return 1 for this agreement |
| Reserved | | | 185 | 1 | | Reserved address |
| Device serial number | | ASC | 186 | 12 | R | |
| Reserved | | | 198 | 2 | | Reserved address |
| Invalid data | | UInt | 200 | 1 | | Internal command |
| Working mode | | UInt | 201 | 1 | R | 0: Power on mode, 1: Standby mode, 2: Mains mode, 3: Off-grid mode, 4: Bypass mode, 5: Charging mode, 6: Failure Mode |
| Mains voltage effective value | 0.1v | Int | 202 | 1 | R | |
| Mains frequency | 0.01Hz | Int | 203 | 1 | R | |
| Average mains power | 1w | Int | 204 | 1 | R | |


