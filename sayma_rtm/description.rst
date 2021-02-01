Project description
===================

The Sayma RTM module extends Sayma AMC board connectivity with DACs and AFE modules.

Functional specifications:
--------------------------

Programmable resources:
^^^^^^^^^^^^^^^^^^^^^^^

* Xilinx Artix-7 XC7A35T-3CSG325E

    * speed grade: -3
    * 4 GTP transceivers (Max Preformance 6.6 Gb/s)

Memory:
^^^^^^^

* EEPROM with MAC and unique ID

Digital to Analog Converters:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* 2 Analog Devices AD9154BCPZ

    * 4 channels
    * 16-bit resolution
    * 2.4 GSPS sampling rate
    * JESD interface

Connectivity:
^^^^^^^^^^^^^

* 2 AFE connectors ASP-134488-01
* RTM connector compatible with Sayma AMC
* 4 GTP transceivers connected to:

	* Sayma AMC FPGA through RTM connector
	* AFE (1 channel to each AFE connector)
	* SATA connector

Clocking:
^^^^^^^^^

Flexible clocking scheme consisting of:

* Si5324 Clock recovery
* Si549 clock generation
* HMC830 + HMC7043 DAC clock and sysref generation
* UFL CLK input and outputs
* SMA CLK input

Other:
^^^^^^

* Temperature monitoring for critical parts of the board
* Power supplied through RTM connector by Sayma AMC

