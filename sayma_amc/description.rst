Project description
===================

The Sayma AMC is a Advanced Mezzanine Card carrier board to carry FMC cards and connect RTM modules. It provides high-speed digital logic and memory.

Functional specifications
-------------------------

Programmable resources:
^^^^^^^^^^^^^^^^^^^^^^^

* Xilinx Kintex UltraScale FPGA - XCKU040-1FFVA-1156C

    * speed grade: -1
    * 20 GTH transceivers (Max Preformance 16.3 Gb/s)

* MMC: LPC17762984

Memory:
^^^^^^^

* 1GB  DDR3 SDRAM with 32-bit interface and 800 MHz clock
* 2GB  DDR3 SDRAM with 64-bit interface and 800 MHz clock
* SPI Flash for FPGA configuration.
* SPI Flash for user data storage
* EEPROM with MAC and unique ID 

Connectivity:
^^^^^^^^^^^^^

* 1 low pin count (LPC) FMC slot for single width mezzanine card
* Micro-USB providing:

    * JTAG
    * AMC FPGA UART
    * RTM FPGA UART
    * MMC UART and DFU

* Stand-alone 12V power connector
* MGT (Multi-Gigabit Transceiver) connected to:

    * RTM x17 (optionally x16 with additional Fat\_Pipe1 connection)
    * Fat\_Pipe1 x1 (optionally x2 without additional RTM connection)
    * Port 0 (additionally connected to SATA)
    * SFP x1 (optionally x2 if Port 0 is connected to ETH PHY)

* RTM connector compatible with Sayma RTM module.

Optional connections can be made by resoldering capacitors on PCB. 

Power supply:
^^^^^^^^^^^^^

* Monitoring of voltage and Power supply for RTM 12V and FMC 12V
* FMC VADJ fixed to 1V8
* Monitoring current of all FMC buses
* Stand-alone power connector

Clocking:
^^^^^^^^^

* Si5324 - clock recovery and jitter attenuator
* uFL CLK input and output
* MCX CLK input and output

Other:
^^^^^^

* Temperature, voltage and current monitoring for critical power buses
* Temperature monitoring: FMC1, supply, FPGA core, DDR memory
* JTAG multiplexer for FMC access, local JTAG port and USB JTAG
* Optional Ethernet RGMII PHY for Port 0

