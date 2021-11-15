Sensors
=======

Both Managment CPU and FPGA can access any of I2C buses. Managment CPU collects all data from all sensors connected to I2C bus. Then the data can be transfered via IPMI to MCH.

Temperature
-----------

+-------+--------+------------------------+----------+-----------+
| No    | Addr.  | Placement              | Type     | Accuracy  |
+=======+========+========================+==========+===========+
| IC34  | 0x4B   | NOR Flash              | LM75     | +/- 2%    |
+-------+--------+------------------------+----------+-----------+
| IC35  | 0x49   | FPGA                   | LM75     | +/- 2%    |
+-------+--------+------------------------+----------+-----------+
| IC36  | 0x4A   | Under VHDCI connectors | LM75     | +/- 2%    |
+-------+--------+------------------------+----------+-----------+
| IC37  | 0x4F   | Near SFPs              | LM75     | +/- 2%    |
+-------+--------+------------------------+----------+-----------+
| IC39  | 0x48   | FPGA                   | MAX664A  | +/- 1%    |
+-------+--------+------------------------+----------+-----------+


All temperature sensors are tied toghether to one I2C bus - SENS\_I2C available on port 5 of I2C switches.

Current
-------

+-------+--------+----------------------+----------+-----------+
| No    | Addr.  | Voltage rail         | Type     | Accuracy  |
+=======+========+======================+==========+===========+
| IC45  | 0x41   | FMC\_P12V0           | INA219   | +/- 0.2%  |
+-------+--------+----------------------+----------+-----------+


All current sensors are tied toghether to one I2C bus - PM\_I2C available on port 5 of I2C switches.

Safety interlocks
-----------------

Temperature interlock is available on RTM board only and gets activated after reaching 80 degrees. This is hardware interlock and cannot be deactivated. Dedicated LED (LD15) gets on power supply is off until the temperature is no longer exceeded.
