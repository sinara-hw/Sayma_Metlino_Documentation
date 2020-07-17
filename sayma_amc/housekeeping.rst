Sensors
=======

Both MMC and FPGA can access any of I2C buses. MMC collects all data from all sensors connected to I2C bus. Then the data can be transfered via IPMI to MCH.

Temperature
-----------

+-------+--------+----------------------+----------+-----------+
| No    | Addr.  | placement            | Type     | Accuracy  |
+-------+--------+----------------------+----------+-----------+
| IC45  | 0x4B   | NOR Flash            | LM75     | +/- 2%    |
+-------+--------+----------------------+----------+-----------+
| IC46  | 0x49   | FPGA                 | LM75     | +/- 2%    |
+-------+--------+----------------------+----------+-----------+
| IC47  | 0x4A   | Under SFPs           | LM75     | +/- 2%    |
+-------+--------+----------------------+----------+-----------+
| IC48  | 0x4F   | power section        | LM75     | +/- 2%    |
+-------+--------+----------------------+----------+-----------+
| IC50  | 0x48   | middle of the board  | MAX664A  | +/- 1%    |
+-------+--------+----------------------+----------+-----------+


All temperature sensors are tied toghether to one I2C bus - SENS\_I2C available on port 6 of I2C switches.

Current
-------

+-------+--------+----------------------+----------+-----------+
| No    | Addr.  | Voltage rail         | Type     | Accuracy  |
+-------+--------+----------------------+----------+-----------+
| IC27  | 0x40   | RTM\_P12V0           | INA219   | +/- 0.2%  |
+-------+--------+----------------------+----------+-----------+
| IC28  | 0x41   | FMC\_P12V0           | INA219   | +/- 0.2%  |
+-------+--------+----------------------+----------+-----------+


All current sensors are tied tohether to one I2C bus - PM\_I2C available on port 5 of I2C switches.

Safety interlocks
-----------------

Temperature interlock is available on RTM board only and gets activated after reaching 80 degrees. This is hardware interlock and cannot be deactivated. Dedicated LED (LD15) gets on and RTM power supply is off until the temperature is no longer exceeded.
