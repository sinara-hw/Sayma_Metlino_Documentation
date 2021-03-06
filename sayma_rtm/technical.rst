PCB technical information
=========================

`Project outputs <https://github.com/sinara-hw/Sayma_RTM/releases>`_

Stackup:

+--------+---------+-------------------------------------------------------------------------------------------+
| Layer  | Copper  | Function                                                                                  |
+--------+---------+-------------------------------------------------------------------------------------------+
| L1     | 36      | polygons, short traces and analog signals between ADC/DAC and connectors                  |
+--------+---------+-------------------------------------------------------------------------------------------+
| P2     | 18      | GND                                                                                       |
+--------+---------+-------------------------------------------------------------------------------------------+
| L3     | 18      | high speed signals, LVDS, GTX, clocks                                                     |
+--------+---------+-------------------------------------------------------------------------------------------+
| P4     | 18      | GND                                                                                       |
+--------+---------+-------------------------------------------------------------------------------------------+
| L5     | 18      | hig speed, LVDS< GTX, clocks, vertical slow control                                       |
+--------+---------+-------------------------------------------------------------------------------------------+
| P6     | 18      | GND                                                                                       |
+--------+---------+-------------------------------------------------------------------------------------------+
| L7     | 18      | non-critical signals like I2C, SPI, mezzanine IOs,                                        |
|        |         | status, LED, some power polygons, etc                                                     |
+--------+---------+-------------------------------------------------------------------------------------------+
| P8     | 18      | GND                                                                                       |
+--------+---------+-------------------------------------------------------------------------------------------+
| P9     | 18      | split power plane                                                                         |
+--------+---------+-------------------------------------------------------------------------------------------+
| L10    | 36      | short signal layers (ADCs), mainly polygons and short traces (EMI mitigation)             |
+--------+---------+-------------------------------------------------------------------------------------------+

The total height of the board is: 1.59 mm. Laminat is a standard FR4.

.. figure:: img/stackup.png

SAYMA RTM stackup
	
.. note:: 
	The thickness of copper in Figure 17 is 0.04mm and 0.02mm is due to approximation. In fact it is 0.036mm and 0.018mm.
