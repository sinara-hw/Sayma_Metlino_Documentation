Project description
===================

The Sayma RTM module extends Sayma AMC board connectivity by DACs and ADCs modules.

Functional specifications:
--------------------------

Programmable resources
^^^^^^^^^^^^^^^^^^^^^^

* Xilinx Artix-7 XC7A15T-1CSG325

Memory:
^^^^^^^

* EEPROM with MAC and unique ID 

Connectivity:
^^^^^^^^^^^^^

* 4x mezzaninne connector LSS-120-01-L-DV-A 
* 40x SMP connector for ADC/DAC
* RTM connector with 16 GTP pair routed to it.
* GTP on RTM connector connected to:

	* DAC x16 [Tx]
	* ADC x8 [Rx]
	* FPGA MGT 2x2 [Tx + Rx]
	* SATA x2 

* uRFB connector

Supply:
^^^^^^^

* Monitoring of voltage and Power supply for FPGA and P3V3

Clocking:
^^^^^^^^^

* UFL CLK input
* SMA CLK output
* Si5324 Clock recovery

Other:
^^^^^^

* Temperature, voltage and current monitoring for critical power buses

