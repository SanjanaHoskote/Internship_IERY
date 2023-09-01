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

  
## FIFO 
FIFO, which stands for First In First Out, is a widely used and valuable design component serving the purpose of synchronization and facilitating communication between modules. 
Depth of FIFO: The depth of a FIFO refers to the number of slots or rows it contains, determining its storage capacity.
Width of FIFO: The width of a FIFO pertains to the number of bits that can be accommodated within each slot or row, defining its data storage capability.

FIFOs come in two primary types:
1. Synchronous FIFO
2. Asynchronous FIFO

These two types of FIFOs play pivotal roles in ensuring orderly data flow and synchronization between components in various digital systems.

## SYNCHRONOUS FIFO 

In Synchronous FIFO, data read and write operations use the same clock frequency. Usually, they are used with high clock frequency to support high-speed systems.

<img width="568" alt="image" src="https://github.com/SanjanaHoskote/Internship_IERY/assets/128903809/650105c6-3ef0-4273-9c43-944c1aa3e8cf"> 

### Synchronous FIFO Operation

Signals:
- wr_en: write enable
- wr_data: write data
- full: FIFO is full
- empty: FIFO is empty
- rd_en: read enable
- rd_data: read data
- w_ptr: write pointer
- r_ptr: read pointer    

**FIFO write operation:**
FIFO can store/write the wr_data at every posedge of the clock based on wr_en signal till it is full. The write pointer gets incremented on every data write in FIFO memory.

**FIFO read operation:**
The data can be taken out or read from FIFO at every posedge of the clock based on the rd_en signal till it is empty. The read pointer gets incremented on every data read from FIFO memory.

**Empty condition**
w_ptr == r_ptr i.e. Write and read pointers has the same value.

**Full condition**
The full condition means every slot in the FIFO is occupied, but then w_ptr and r_ptr will again have the same value. Thus, it is not possible to determine whether it is a full or empty condition. 
Thus, the last slot of FIFO is intentionally kept empty, and the full condition can be written as ((w_ptr+1’b1) == r_ptr)

### Synchronous FIFO Verilog Code
[Design](https://github.com/SanjanaHoskote/Internship_IERY/blob/main/Synchronous%20FIFO.v)
[Testbench](https://github.com/SanjanaHoskote/Internship_IERY/blob/main/Synchronous%20FIFO.v)

### Simulation Results

<ss>

## ASYNCHRONOUS FIFO

In asynchronous FIFO, data read and write operations use different clock frequencies. Since write and read clocks are not synchronized, it is referred to as asynchronous FIFO. Usually, these are used in systems where data need to pass from one clock domain to another which is generally termed as ‘clock domain crossing’. Thus, asynchronous FIFO helps to synchronize data flow between two systems working on different clocks.

<img width="608" alt="asyncfifo" src="https://github.com/SanjanaHoskote/Internship_IERY/assets/128903809/66f921be-4309-496b-a2fe-962429a6c9fe">


### Asynchronous FIFO Operation

**Signals:**
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

**Write Operation:** In the case of an asynchronous FIFO, the write operation is associated with the write clock domain. This means that the write pointer, which tracks the location where data is being written into the FIFO, is aligned with the write clock's timing.

**Read Operation:** Conversely, the read operation in an asynchronous FIFO is synchronized with the read clock domain. In this context, the read pointer, responsible for identifying the position from which data is being read, is aligned with the read clock's timing.

**Metastability Challenge:** Managing asynchronous FIFOs introduces a critical challenge related to metastability. The fact that write and read pointers operate in separate clock domains necessitates a mechanism for safely transferring data and status information between these domains.

**Mitigating Metastability:** To address this concern, we employ synchronizers, which are typically implemented using flip-flops. Specifically, we opt for a "2 flip-flop synchronizer" design. However, it's important to note that a single "2FF synchronizer" can effectively resolve metastability for just one bit.

**Multiple Synchronizers:** Given that both write and read pointers comprise multiple bits, we must employ multiple instances of the "2FF synchronizer." Each synchronizer handles a distinct bit of the pointers.

<img width="473" alt="2ffsynch" src="https://github.com/SanjanaHoskote/Internship_IERY/assets/128903809/5f7ad223-30ef-4e13-98bb-43db05b8267b">

**Full and Empty Conditions:**
Efficiently detect full and empty conditions directly using gray-coded write and read pointers, eliminating the need for additional hardware to convert them into binary form.

Full condition - wfull = (g_wptr_next == {~g_rptr_sync[PTR_WIDTH:PTR_WIDTH-1], g_rptr_sync[PTR_WIDTH-2:0]});

Empty condition - rempty = (g_wptr_sync == g_rptr_next);

### Asynchronous FIFO Verilog Code
- [Design](https://github.com/SanjanaHoskote/Internship_IERY/blob/main/Asynchronous%20FIFO.v)
- [Testbench](https://github.com/SanjanaHoskote/Internship_IERY/blob/main/Synchronous%20FIFO.v)

### Simulation Results

<ss>

## Communication protocols 
## SPI - Serial Peripheral Interface

The Serial Peripheral Interface, SPI, is an interface specification for synchronous serial data transfer, using a clock.
- SPI supports half/full duplex serial communication.
- It is synchronous communication protocol.
- Four wire communication protocol.
- Most cases, one master multiple slave protocol but also supports multiple master configuration
- It supports max speed up to 10Mbps
- Master provides clock for synchronisation
- It supports 8 and 16 bit format.
  
 ### Block Diagram
 <img width="188" alt="image" src="https://github.com/SanjanaHoskote/Internship_IERY/assets/128903809/65e56af4-4552-4269-89fe-7d39897e3d5c">
 <img width="240" alt="image" src="https://github.com/SanjanaHoskote/Internship_IERY/assets/128903809/c4ba84bd-5251-4c66-a227-c2636c48e2e4">

 ### Timing diagram / CPOL, CPHA
 <img width="401" alt="image" src="https://github.com/SanjanaHoskote/Internship_IERY/assets/128903809/c221bcdb-a943-4f73-9cfa-142f86a9b6dd">

 ### State Diagram 
<img width="784" alt="image" src="https://github.com/SanjanaHoskote/Internship_IERY/assets/128903809/e5e43bca-04c1-4011-a5d1-4bcddc355f4e">
 
### SPI Verilog Code
- [Design](https://github.com/SanjanaHoskote/Internship_IERY/blob/main/SPI.v)
- [Testbench](https://github.com/SanjanaHoskote/Internship_IERY/blob/main/SPI%20tb.v)

### Simulation results
<img width="784" alt="image" src="https://github.com/SanjanaHoskote/Internship_IERY/assets/128903809/21aeda57-bdb4-4dc7-b663-a22329d88afb">







