Sayma
=====

Sayma is a hardware that supports M-Lab's Smart Arbitrary Waveform Generator (`SAWG <http://m-labs.hk/artiq/manual-master/core_drivers_reference.html?highlight=sawg#module-artiq.coredevice.sawg>`_) gateware.
Provide 8 channels of 1.2 GSPS 16-bit DACs (2.4 GHz DAC clock) and 125 MSPS 16-bit ADCs. It
consists of an AMC, providing the high-speed digital logic, and a RTM,
holding the data converters and analog components.

The design files are located in
`Sayma AMC <https://github.com/sinara-hw/Sayma_AMC/>`_
and
`Sayma RTM <https://github.com/sinara-hw/Sayma_RTM/>`_
repositories. PDF schematics are located in releases section of each repository.
The PCBs are double width, mid height AMC module. Sayma AMC

Features
--------

* May be used in a uTCA rack or stand-alone operation with fibre-based
  DRTIO link
* Analog input and output front-ends provided by plug-in
  `AFE <https://github.com/sinara-hw/meta/wiki/SaymaAFE>`_ modules (eg. BaseMod) for maximum
  flexibility.
* Extremely flexible `clocking options <https://github.com/m-labs/sinara/wiki/SinaraClocking>`_
* Flexible feedback to SAWG parameters planned. Specification
  `here <https://github.com/m-labs/sinara/wiki/Servo>`_.

Key AMC Components
------------------

Programmable resources:
^^^^^^^^^^^^^^^^^^^^^^^

* Xilinx Kintex UltraScale – XCKU040-1FFVA-1156C FPGA 20 I/O, 530K Logic Cells

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
		* Fat_Pipe1 x2
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

* Clock recovery Si5324  is a precision clock multiplier andjitter attenuator
* UFL CLK input
* SMA CLK output

Other:
^^^^^^

* Temperature, voltage and current monitoring for critical power buses
* Temperature monitoring: FMC1, supply, FPGA core, DDR memory
* JTAG multiplexer (SCANSTA) for FMC access, local JTAG port and remote debug/Chipscope via Ethernet

Key RTM Components
------------------
.. warning::
    not sure if it should be here? only in RTM doc?

*  **DAC**: AD9154 4-channel high-speed data converter

  * data rate is 1.2 GS/s at 16-bit
  * clock is up to 2.4 GHz (1x, 2x, 4x and 8x interpolating modes)
  * supports mix-mode to emphasize power in 3rd Nyquist Zone
  * interface is 8-lane JESD204B (subclass 1)
  * power consumption is 2.11 W
  * each Sayma has 2 AD9154

* **Clock generation**: (summarized `here <https://github.com/m-labs/sinara/wiki/SinaraClocking>`_)

    * Sayma has several distinct clock domains

        * DAC, JESB204B output clock
        * LO for analog mezzanines

    * These clocks may be generated using a low phase noise
    \href{ClockMezzanines}{Clock Mezzanine} PCB. A single Clock
    Mezzanine can be shared by several Sayma in a uTCA crate using
    {[}Baikal{]} PCB and an RTM RF backplane. Alternately, each Sayma
    can have its own distinct Clock Mezzanine (local generation).

* **Clock distribution**

    * HMC7043 SPI 14-Output Fanout Buffer for JESD204B
    * HMC830 SPI fractional-N PLL

