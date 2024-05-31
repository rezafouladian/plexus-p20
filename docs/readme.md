# Technical information and documentation of Plexus machines

For the Plexus P/20 and P/15:

- MC1488 / MC1489 used at RS232 drivers/buffers
- MOSTEK MK68564 used as the SIO chip to drive the 8 serial ports (4 of them)
- The Motorola MC146818 is used for the battery backed up RAM and clock, just like on the PC and countless other systems. There is a NiCD battery attached to the motherboard to power this circuit when the system is turned off.  
- The reset circuit on the Plexus is inside the primary power supply. Pin 4 on the motherboard power connector is the input from the PSU to the motherboard, so you cannot run the system on the bench with a different power supply without managing this input yourself. (Logic low = held in reset, logic high 5V = out of reset.) 
- The PSU remote power on pin is grounded to turn on the power supply. It has relays inside to turn itself on and also power up the secondary PSU and external fan.
- The PSU reset input is grounded by the front keyswitch, which which is input into the reset circuit inside the PSU which supplies a reset (Logic low) to the motherboard via pin 4.
- On my system the primary PSU powers up the motherboard, hard drive, floppy drive and tape drive. The secondary PSU powers up the multibus backplane and the SCSI / MFM interposer. These are powered by normal Molex style 5/12v connectors, so you can use a normal PC splitter to eliminate the secondary PSU if you aren't using any multibus cards. Just unplug the cable that feeds from the primary to the secondary power supply.
- Serial port 0 is the console port. See below for the pinout. (It is non standard)
- The top processor (MC68010) runs the boot diagnostics. Without it installed or working, the system will not do anything at power up.
- My system has 2048k of 41256 DRAM installed (8 chips + 1 parity per 256k) on a RAM board on the backside of the motherboard. All of the RAM chips are in sockets and can be tested in another system. 

### Serial port map:

(Thanks to Patron Peter)

The book "Complete Guide to Rs-232 and Parallel Connections: A Step-By-Step Approach to Connecting Computers, Printers, Terminals, and Modems" actually has the serial connector pinouts for the Plexus P/15 and P/20.
```
Port 0 - console
port 1-3 no HW handshake
port 4-5 signals provided for modems
port 6-7 include timing lead support

Ports 1-3: 1-TX, 6-RX and 8-GND
Ports 4-5: 1-TX, 2-RTS, 4-DTR, 6-RX, 7-CTS, 8-GND, 9-DCD
Port 6-7: Normal DB25 RS232 with full flow control
```
Memory map:
```
|   Bus Master | Space |  Loc   | Address Range | Function
 Job MB DMA BLK Usr Sys Job DMA
  x      x       x       x       000000 - 7FFFFF  Main User memory
  x      x           x   x       000000 - 7FFFFF  Main Sys memory
     x               x   x       780000 - 7FFFFF  Main Sys Memory
             x       x   x       600000 - 6FFFFF  Main Sys Memory
  x      x           x       x   800000 - 80FFFF  PROM (4 - 27128s)
  x                  x   x       900000 - 901FFF  User Page Map
  x                  x   x       902000 - 903FFF  Sys Page Map
  x      x           x       x   A00001 - A0003F  UART 'A' (odd)
  x      x           x       x   A10001 - A1003F  UART 'B' (odd)
  x      x           x       x   A20001 - A2003F  UART 'C' (odd)
  x      x           x       x   A30001 - A3003F  UART 'D' (odd)
  x      x           x       x   A70000 - A70003  SCSI Data Buffers
  x                  x   x       B00000 - B7FFFF  MBus I/O Space
  x                  x   x       B80000 - BFFFFF  MBus Mem Space
  x      x   x       x       x   C00000 - C03FFF  SRAM
  x      x           x       x   D00001 - D0001B  Calendar (odd)
  x      x           x       x   D0001D - D0007F  Calendar RAM (odd)
  x      x           x       x   E00000 - E0001E  Control/Status Reg.
  x      x           x       x   F00003           MBus Int Vector
  x      x           x       x   F00005           Job Int Vector
  x      x           x       x   F00007           DMA Int Vector
  x      x           x       x   F00009           SCSI Int Vector
  x      x           x       x   F0000B           UART Int Vector
  x      x           x       x   F0000D           Clock Int Vector
  x      x           x       x   F0000F           Panic Int Vector
```
