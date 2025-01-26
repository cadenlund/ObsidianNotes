### RAM
*  Giga means 10^9 or about a billion
	A byte is 8 bits
* Made up of zeros and ones, the physical computer uses transistors with charges to represent 0 and 1
* slots in ram can be immediately accessed
* Data is represented as bytes so storing it requires converting is to bytes
* RAM is made up of a large, contiguous array of memory cells 
* Each cell can hold 1 byte of data (8 bits)
* Every single byte has a unique address that identifies its location in the memory. 
* The address is 32 bits long (4 bytes) in 32 bit architecture and 64 bits (8 bytes) in 64 bit architecture. Used by the CPU when the byte is needed to load into cache.  The value of the address just ticks up by 1 for the length of the address. 32 byte systems could only hold 4gb of information 
* The addresses are handled and stored by the processor and [[memory management unit]]. 

| Address (Key)   | Value (Byte)         |
|------------------|----------------------|
| `0x00000000`     | `01101101` (109 in decimal) |
| `0x00000001`     | `00111100` (60 in decimal)  |
| `0x00000002`     | `11110000` (240 in decimal) |
| `0x00000003`     | `00001111` (15 in decimal)  |

