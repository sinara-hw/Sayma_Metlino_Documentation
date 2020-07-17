USB-UART
========

* PRI\_UART is connected to FPGA, configuration is 1N8 115200 baudrate
* AUX\_UART is connected to FPGA, configuration is 1N8 115200 baudrate (this UART is usually routed to RTM through FPGA)
* MMC\_CONS\_PROG is connected to MMC, configuration is 1N8 115200 baudrate

USB-UART bridge
---------------

Usually on Windows 10 and Linux no additional drivers are needed for the USB-UART bridge (FT4232H). Please refer to the `FTDI website <http://www.ftdichip.com>`_ in case of missing drivers in your system. Drivers allow the user to communicate with the USB interface via a standard PC serial emulation port (TTY).

