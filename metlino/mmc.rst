
.. _metlino_cpu:

Managment CPU
=============

Managment CPU configures and manages various aspects of Metlino to operate correctly in MicroTCA crate. Its main tasks are:

* communication with the MCH,
* negotiating payload power,
* enabling various on-board power supplies,
* monitoring stability of on-board power supplies,
* managing hotswap abilities,
* supplying sensor information via IPMB.

Managment CPU boot
------------------

During boot Managment CPU performs several tasks:

* configures CPU, UART from own FLASH
* sets IO port directions
* configures I2C switch base address for master ports - MMC, FPGA
* communicates with MCH and asks for payload power if hotswap handle is inserted
* enables power supplies in sequence described in power supply section

.. note::
	Configuration of CPU, UART does not affect any of LED indicators.

SW2 header
----------

SW2 header can be used to enable ISP mode of Managment CPU processor. Enabling first and second switch allows to program the processor using USB. Enabling last switch allows to run the board outside of the crate.

See :ref:`metlino_overview` for the location of the header (top view, call-out 16).

Bootstraping
------------
To compile binaries newest version of `LPCXpresso <https://www.nxp.com/products/processors-and-microcontrollers/arm-based-processors-and-mcus/lpc-cortex-m-mcus/lpc1100-cortex-m0-plus-m0/lpcxpresso-ide-v8.2.2:LPCXPRESSO?tab=Design_Tools_Tab>`_ or `MCUXpresso <https://www.nxp.com/design/software/development-software/mcuxpresso-software-and-tools-/mcuxpresso-integrated-development-environment-ide:MCUXpresso-IDE>`_ is needed.

Another option is to compile under Linux using cmake toolchain.

::

	mkdir build && cd build
	cmake ../ -DBOARD=metlino -DTARGET_CONTROLLER=LPC1776 -DCMAKE_BUILD_TYPE=Debug
	make

Minimal Dockerfile which downloads and compiles firmware:

::

	FROM ubuntu:20.04

	RUN DEBIAN_FRONTEND=noninteractive apt-get update && \
		DEBIAN_FRONTEND=noninteractive apt-get upgrade -y && \
		DEBIAN_FRONTEND=noninteractive apt-get install -y gcc-arm-none-eabi cmake git

	RUN git clone https://github.com/sinara-hw/openMMC.git && \
		cd openMMC && git checkout sayma-devel # Update link to metlino
	
	ENV CC="arm-none-eabi-gcc"
	ENV CXX="arm-none-eabi-g++"

	RUN cd openMMC && mkdir build && cd build && \ 
		cmake ../ -DBOARD=metlino -DTARGET_CONTROLLER=LPC1776 \
			-DCMAKE_BUILD_TYPE=Debug -DCMAKE_C_COMPILER_WORKS=1 \
			-DCMAKE_CXX_COMPILER_WORKS=1 && \
		make

	ENTRYPOINT ["/bin/bash"]

You can use it as a reference to configure your system for compilation.

Source code of OpenMMC is located `here <https://github.com/sinara-hw/openmmc/tree/sayma-devel>`_. Currently "sayma-devel" branch is used.

.. todo::
		* Add steps to import project to LPCXpresso
		* Add steps for MCUXpresspo
		* Publish Metlino firmware repository

Header flashing
^^^^^^^^^^^^^^^

The Managment CPU can be upgraded by USB cable and NXP programmer (can be used other programmer but make sure that header shorts pins 3, 5, 9) using LPCXpresso/MCUXpresso, `Flashmagic <http://www.flashmagictool.com/>`_ or any other software which can talk with NXP bootloader. The tested programmer is LPCLink V2. Flashing using programmer also allows to debug the CPU. Use ``openMMC.axf`` file for flashing with LPCXpresso/MCUXpresso.

.. todo:: Add photo or diagram showing correct ribbon connection with programmer.

USB flashing
^^^^^^^^^^^^

The Managment CPU can be upgraded using USB and flashmagic software. This option only allows to flash IC, without any debug option.
Steps to flash using USB:

	* Set serial console 115200 8n1
	* Press front-panel button PB3 to trigger MMD to dump to serial console
	* Set LPC1776, 8MHz oscillator
	* Select all flash blocks for erase along with CodeRd Prot
	* Select hex file 
	* Enable verify after programming
	* Press start

.. todo::
	* Verify flashing with flashmagic (steps above were checked for Sayma v1.0, there may be need to set switch)

