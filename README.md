# Internship_IERY
This repository consists of all the prerequisites of the main AXI to APB project done as a part of our internship. We have designed and implemented it in Verilog. 
These include :
- Synchronous FIFO
- Asynchronous FIFO
-  Basic communication protocols:
   - SPI
   - I2c
   - UART
# Quick links :
- Files:
  - [Synchronous FIFO RTL](https://github.com/SanjanaHoskote/Internship_IERY/blob/main/Synchronous%20FIFO.v)
  - Synchronous FIFO testbench
  - [Asynchronous FIFO RTL](https://github.com/SanjanaHoskote/Internship_IERY/blob/main/Asynchronous%20FIFO.v)
  - Asynchronous FIFO testbench
  - [SPI RTL](SPI.v)
  - [SPI testbench](https://github.com/SanjanaHoskote/Internship_IERY/blob/main/SPI%20tb.v)
  - [I2C RTL](https://github.com/SanjanaHoskote/Internship_IERY/blob/main/i2c_master.v)
  - [I2C testbench](https://github.com/SanjanaHoskote/Internship_IERY/blob/main/i2c_master_tb.v)
  - UART RTL
  - UART testbench
  
- Navigation through the report
**FIFO**  
FIFO, which stands for First In First Out, is a widely used and valuable design component serving the purpose of synchronization and facilitating communication between modules. 
Depth of FIFO: The depth of a FIFO refers to the number of slots or rows it contains, determining its storage capacity.
Width of FIFO: The width of a FIFO pertains to the number of bits that can be accommodated within each slot or row, defining its data storage capability.

FIFOs come in two primary types:
1. Synchronous FIFO
2. Asynchronous FIFO

These two types of FIFOs play pivotal roles in ensuring orderly data flow and synchronization between components in various digital systems.

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

FIFO write operation:

FIFO can store/write the wr_data at every posedge of the clock based on wr_en signal till it is full. The write pointer gets incremented on every data write in FIFO memory.

FIFO read operation: 

The data can be taken out or read from FIFO at every posedge of the clock based on the rd_en signal till it is empty. The read pointer gets incremented on every data read from FIFO memory.

<img width="549" alt="image" src="https://github.com/SanjanaHoskote/Internship_IERY/assets/128903809/bd0fd89e-b515-4673-a521-997943ac6318">


ASYNCHRONOUS FIFO

In asynchronous FIFO, data read and write operations use different clock frequencies. Since write and read clocks are not synchronized, it is referred to as asynchronous FIFO. Usually, these are used in systems where data need to pass from one clock domain to another which is generally termed as ‘clock domain crossing’. Thus, asynchronous FIFO helps to synchronize data flow between two systems working on different clocks.

Signals:

- wr_en: write enable
- wr_data: write data
- full: FIFO is full
- empty: FIFO is empty
- rd_en: read enable
- rd_data: read data
- b_wptr: binary write pointer
- g_wptr: gray write pointer
- b_wptr_next: binary write pointer next
- g_wptr_next: gray write pointer next
- b_rptr: binary read pointer
- g_rptr: gray read pointer
- b_rptr_next: binary read pointer next
- g_rptr_next: gray read pointer next
- b_rptr_sync: binary read pointer synchronized
- b_wptr_sync: binary write pointer synchronized

Asynchronous FIFO Operation

In the case of synchronous FIFO, the write and read pointers are generated on the same clock. However, in the case of asynchronous FIFO write pointer is aligned to the write clock domain whereas the read pointer is aligned to the read clock domain. Hence, it requires domain crossing to calculate FIFO full and empty conditions. This causes metastability in the actual design. In order to resolve this metastability, 2 flip flops or 3 flip flops synchronizer can be used to pass write and read pointers. For explanation, we will go with 2 flip-flop synchronizers. Single “2 FF synchronizer” can resolve metastability for only one bit. Hence, depending on write and read pointers multiple 2FF synchronizers are required.



<img width="560" alt="image" src="https://github.com/SanjanaHoskote/Internship_IERY/assets/128903809/6ff381a6-cc6a-4333-acfb-008fcb16b864">

