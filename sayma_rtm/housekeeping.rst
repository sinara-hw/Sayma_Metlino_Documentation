Housekeeping Signals
====================

Sensors
-------

Temperature:

IPMI\_I2C:

+-------+--------+-----------------------------------+----------+-----------+
| No    | Addr.  | placement                         | Type     | Accuracy  |
+-------+--------+-----------------------------------+----------+-----------+
| IC39  | 0x4A   | Bottom - close to ADC             | LM75     | +/- 2     |
+-------+--------+-----------------------------------+----------+-----------+
| IC40  | 0x49   | Bottom - close to HMC7043 buffer  | LM75     | +/- 2     |
+-------+--------+-----------------------------------+----------+-----------+
| IC44  | 0x24   | Bottom - under Mezzanine1         | MAX664A  | +/- 1     |
+-------+--------+-----------------------------------+----------+-----------+

TEMP\_SENS\_I2C:

+-------+--------+---------------------+----------+-----------+
| No    | Addr.  | placement           | Type     | Accuracy  |
+-------+--------+---------------------+----------+-----------+
| IC56  | 0x48   | Bottom - under DAC  | ADT7420  | +/- 0.25  |
+-------+--------+---------------------+----------+-----------+
| IC57  | 0x49   | Bottom center       | ADT7420  | +/- 0.25  |
+-------+--------+---------------------+----------+-----------+

All sensors are available either from AMC side I2C or from RTM\_FPGA.

Safety interlocks
-----------------

Temperature interlock is available on RTM board only and gets activated after reaching 80 degrees. This is hardware interlock and cannot be deactivated. Dedicated LED (LD15) gets on and RTM power supply is off until the temperature is exceeded.
