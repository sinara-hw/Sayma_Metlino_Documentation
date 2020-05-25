USB-UART
========

USB console switch
------------------

UART from FPGA is connected through Multiplexer (SN74CB3T3257PW). Selection between MMC and USB is preformed automaticly. When micro-USB is connected, +5V from USB bus  switches the multiplexer to pass data from USB to FPGA, after un plugging cable, the switch signal S fall down and the multiplexer conects MMC to FPGA.

* PRI\_UART is connected to FPGA, configuration is 1N8 115200 baudrate
* AUX\_UART is connected to FPGA, configuration is 1N8 115200 baudrate
* UART1 is connected to MMC, configuration is 1N8 115200 baudrate
* UART4 is connected to MMC, configuration is 1N8 115200 baudrate
* MMC\_CONS\_PROG is conected to MMC, configuration is 1N8 115200 baudrate

USB-UART bridge
---------------

The USB-UART bridge (FT4232H) requires USB device drivers, vailable free from http://www.ftdichip.com,
which are used to make the FT4232H on the Mini Module appear as a four virtual COM ports (VCP). This
then allows the user to communicate with the USB interface via a standard PC serial emulation port
(TTY).

