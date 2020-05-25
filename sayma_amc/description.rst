Project description
===================

The Sayma AMC is a Advanced Mezzanine Card carrier board to carry FMC cards and connect RTM modules.

Functional specifications
-------------------------

Programmable resources:
^^^^^^^^^^^^^^^^^^^^^^^

* Xilinx Kintex UltraScale – XCKU040-1FFVA-1156C FPGA

    * speed grade: -1
    * 20 GTH transceivers (Max Preformance 16.3 Gb/s)

* MMC: LPC17762984

Memory:
^^^^^^^

* 512Mb  DDR3 SDRAM (32-bit interface), 800MHz (clock)
* 1Gb  DDR3 SDRAM (64-bit interface), 800MHz (clock)
* SPI Flash for FPGA configuration. Accessible by MMC
* SPI Flash for user data storage
* EEPROM with MAC and unique ID 

Connectivity:
^^^^^^^^^^^^^

* 1 high pin count (HPC) FMC slot for single width mezzanine card
* Micro-USB UART connected to FPGA or MMC
* Stand-alone 12V power connector 
* MGT (Multi-Gigabit Transceiver) connected to:

    * FMC x1
    * RTM x16
    * Fat\_Pipe1 x2
    * AMC P2P x4
    * Port 0 – possibility connected to SATA
    * SFP x2

* RTM connector with 8 GTP routed to it. Compatible with Sayma RTM module.
* Port 0 – possibility connected to SATA
* RTM connector compatible with Sayma RTM module

Supply:
^^^^^^^

* Monitoring of voltage and Power supply for RTM 12V and FMC 12V
* FMC VADJ fixed to 1V8
* Monitoring current of all FMC buses
* Stand-alone power connector

Clocking:
^^^^^^^^^

* Clock recovery Si5324
* UFL CLK input
* SMA CLK output

Other:
^^^^^^

* Temperature, voltage and current monitoring for critical power buses
* Temperature monitoring: FMC1, supply, FPGA core, DDR memory
* JTAG multiplexer (SCANSTA) for FMC access, local JTAG port and remote debug/Chipscope via Ethernet

