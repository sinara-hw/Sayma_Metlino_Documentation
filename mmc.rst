MMC
===

MMC steps during booting
------------------------

* configures CPU, UART from own FLASH 
* sets IO port directions
* enables VCCINT PSU
* enables P5V0 PSU (helper PSU)
* enables Exar PSU. It boots from its own EEPROM
* waits 200ms
* configures SCANSTA chip in stitcher mode. If RTM is inserted, it enables its JTAG port
* configures I2C switch base address for master ports - MMC, FPGA
* initializes default RTM power state to off
* initializes Ethernet PHY chip in RGMII mode using pin strap.
* waits 200ms
* initializes I2C controller and chain (switch)
* configures Si5324
* checks if RTM is inserted, if yes, then enables its power, waits 200ms and initializes RTM power supply via RTM\_I2C. It also configures Si5324 on RTM
* runs task.
 
The task performs following functions:

* checks if FPGA is configured. If not, it keeps Ethernet PHY in reset state. Once FPGA gets configured, it initializes the PHY.
* checks if RTM is unplugged. If not, it switches the power off to make sure it is off during hotplug.

.. note::
	Configuration CPU, UART does not affect with any changes of LED indicators.

Bootstraping
------------
To compile binaries `LPCXpresso <https://www.nxp.com/products/processors-and-microcontrollers/arm-based-processors-and-mcus/lpc-cortex-m-mcus/lpc1100-cortex-m0-plus-m0/lpcxpresso-ide-v8.2.2:LPCXPRESSO?tab=Design_Tools_Tab>`_ in newest version is needed.

`LPCXpresso User Guide <https://www.nxp.com/docs/en/user-guide/LPCXpresso_IDE_User_Guide.pdf>`_

Another option is to compile under Linux using cmake toolchain in version 4.9.3. ::

	cmake & arm-none-eabi-gcc

Header flashing
^^^^^^^^^^^^^^^

The MMC can be upgraded by USB cable and NXP programmer (can be used other programmer but make sure that header shorts pins 3, 5, 9) using `Flashmagic <http://www.flashmagictool.com/>`_ or any other software which can talk with NXP bootloader. The tested programmer is LPCLink V2. Flashing using programmer allows to debug.

USB flashing
^^^^^^^^^^^^

The MMC can be upgraded using USB and flashmagic software. This option only allows to flash IC, without any debug option.
Steps to flash using USB:

	* Set serial console 115200 8n1
	* Press front-panel button -PB3 to trigger MMD to dump to serial console
	* Set LPC1776, 8MHz oscillator, select hex file and press start

The source code is written in C and can be found on github.\\ 
`Source code <https://github.com/m-labs/sinara/tree/master/SAYMA\_firmware>`_
\textbf{`Pre-compiled binary <https://github.com/m-labs/mmc-firmware/releases>`_

Functionality
-------------

.. todo::
	needed?

Exar debugging
--------------
 
In case of chip failure, i.g.overvoltage, overcurrent, etc., there is possibility  to check chip status via UART. In this case in UART console, Exar register readout can be done by typing 'P' character.

.. figure:: img/exarreg.png

Exar register

PHY debugging
-------------

In case of chip failure, there is possibility to check chip status via UART. In this case in UART console, Ethernet PHY content can be read by typing 'E' character

.. figure:: img/phyreg.png

Ethernet PHY register

Ethernet
--------

There is no Ethernet sharing between MMC and FPGA due to speed difference between RGMII and RMII. PHY does not provide link speed translation.

OpenMMC
-------

`OpenMMC Project <https://github.com/lnls-dig/openMMC>`_

.. todo::
	TBD
