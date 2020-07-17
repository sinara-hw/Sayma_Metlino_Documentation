USB-UART
========

* ``PRI\_UART`` is connected to FPGA, configuration depends on FPGA (usually 8N1 115200 baudrate)
* ``AUX\_UART`` is connected to FPGA, configuration depends on FPGA (this UART is usually routed to RTM through FPGA, usual configuration: 8N1 115200 baudrate)
* ``MMC\_CONS\_PROG`` is connected to MMC, configuration is 8N1 115200 baudrate (again, can be changed by changing settings in MMC firmware)

USB-UART bridge
---------------

Usually on Windows 10 and Linux no additional drivers are needed for the USB-UART bridge (FT4232H). Please refer to the `FTDI website <http://www.ftdichip.com>`_ in case of missing drivers in your system. Drivers allow the user to communicate with the USB interface via a standard PC serial emulation port (TTY).

These UART console appear usually as ``/dev/ttyUSB1`` through ``/dev/ttyUSB3`` on Linux if no other ttyUSB devices are connected (``ttyUSB1`` - ``PRI\_UART``, ``ttyUSB2`` - ``AUX\_UART``, ``ttyUSB3`` - ``MMC\_CONS\_PROG``). ``/dev/ttyUSB0`` is in this case a JTAG connection, usually used by openocd. On Windows with correct drivers, FTDI usually appears as 4 consecutive COM ports. The latter 3 are ``PRI\_UART``, ``AUX\_UART`` and ``MMC\_CONS\_PROG``.
