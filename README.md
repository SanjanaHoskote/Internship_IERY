# Internship_IERY
This repository consists of all the prerequisites and the main axi to apb project done as a part of our internship.

FIFO
First In First Out (FIFO) is a very popular and useful design block for purpose of synchronization and a handshaking mechanism between the modules.
Depth of FIFO: The number of slots or rows in FIFO is called the depth of the FIFO.

Width of FIFO: The number of bits that can be stored in each slot or row is called the width of the FIFO.

There are two types of FIFOs:
1) Synchronous FIFO
2) Asynchronous FIFO

SYNCHRONOUS FIFO 

In Synchronous FIFO, data read and write operations use the same clock frequency. Usually, they are used with high clock frequency to support high-speed systems.

Synchronous FIFO Operation
Signals:
- wr_en: write enable
- wr_data: write data
- full: FIFO is full
- empty: FIFO is empty
- rd_en: read enable
- rd_data: read data
- w_ptr: write pointer
- r_ptr: read pointer    

FIFO write operation
FIFO can store/write the wr_data at every posedge of the clock based on wr_en signal till it is full. The write pointer gets incremented on every data write in FIFO memory.

FIFO read operation
The data can be taken out or read from FIFO at every posedge of the clock based on the rd_en signal till it is empty. The read pointer gets incremented on every data read from FIFO memory.

<img width="549" alt="image" src="https://github.com/SanjanaHoskote/Internship_IERY/assets/128903809/bd0fd89e-b515-4673-a521-997943ac6318">


ASYNCHRONOUS FIFO

<img width="560" alt="image" src="https://github.com/SanjanaHoskote/Internship_IERY/assets/128903809/6ff381a6-cc6a-4333-acfb-008fcb16b864">

