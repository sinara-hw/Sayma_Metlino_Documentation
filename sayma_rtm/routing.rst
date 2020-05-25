Routing
=======

This section contain general block schematic of SAYMA RTM board and I2C map with addresses. General Block Schematic -figure \ref{BlockScheme} shows more important connections between components. I2C connections with addresses can be found in figure \ref{I2C}.

.. figure:: img/sch.eps

Block Schematic

I2C
---

The I2C MUX is made from two (TCA9548ARGER)  I2C multiplexers. In Sayma AMC there are two main I2C busses: MMC\_I2C and FPGA\_I2C. Each of them is connected to one multiplexer. Outputs are tied together, so Masters (MMC and FPGA) can acces to any of 7 I2C busses. Addidtionaly MMC has acces to FPGA\_I2C and is connected to IPMB through AMC connector.

.. figure:: img/i2c.eps

I2C

