Power
=====

Power supply
------------
The card can operate connected to SAYMA AMC card. When the card is inserted to the SAYMA AMC,  the power is applied from RTM connector -J2, +12V (4 power lines) and +3.3\_MP(1 line). Both voltages can be controlled by SAYMA AMC.

+12 is converted to lower voltages and negative voltages, simplified power map is in figure \ref{powermap}. There are few types of power distributors, fixed LDOs, Buck converters and Exar chip - quad channel digital Pulse Width Modulated (PWM) step down (buck) controller. Exar allows for adjust power parameters and for set particular power oorder. Exar firmware monitors current and responds if its too high.  Additionaly all LDOs have connected PowerGood outputs so it is possible to read the proper state of all power busses.

.. TODO
    TBD voltage noise

Maximum board(AMC+RTM module) power consumption is estimated to 6A @ 12V.

.. Note::
    Power consumption mostly depends on FPGA configuration.

* Input voltage range: 10.8-13.2 [V]
* The board needs active cooling. Approx. 20CFM in 20 C air.

Power configuration
-------------------

Power map
^^^^^^^^^

.. figure:: img/pwr.eps

Power map

+---------+---------+-----+
| P1V0    | 1.0V    | 4A  |
+---------+---------+-----+
| P1V2    | 1.2V    | 2A  |
+---------+---------+-----+
| P1V8    | 1.8V    | 2A  |
+---------+---------+-----+
| P2V0    | 2.0V    | 4A  |
+---------+---------+-----+
| P3V3    | 3.3V    | 4A  |
+---------+---------+-----+
| P4V0    | 4.0V    | 4A  |
+---------+---------+-----+
| P6V0    | 6.0V    | 8A  |
+---------+---------+-----+
| P12V0A  | 12.0V   | 1A  |
+---------+---------+-----+
| N6V0A   | -6.0V   | 4A  |
+---------+---------+-----+
| N12V0A  | -12.0V  | 1A  |
+---------+---------+-----+

Exar parameters
^^^^^^^^^^^^^^^

Exar chip(XR77129) has 4 configurable outputs with configirable current limits. Each channel can be confugured individually. It is possible to set voltage, current limit and power sequencing. In Figure \ref{exar1} we can see that main power supply is 12V, Under Voltage Lockout(UVLO) is set to 6V, so below this value chip will shutdown all channels. When the temperature rise under Over Temperature Protection (OTP) 105 degrees, the chip will generate warning event and restart.

All 4 channells can be grouped together and will start-up and shut-down in an user defined sequwnce. Selecting none means channels will not be assigned to any group and therefore, will be controlled independently. Group 0 is controlled by ENABLE or EXAR\_I2C command. Group 1 can be controlled by GPIO or by EXAR\_I2C command. By selecting 'Wait PGOOD' next channel will not power up untill current channel reaches the target level. Delay is an additional delay time which postpone after power up one and another channel in group.

In on-going Exar configuration - Figure \ref{exar2}, power sequencing looks like this: After Enable - EN\_DC\_DC, P1V0 start-up, after it reaches proper value, Exar turn ramp up another channel in this group - P3V3. In the same order are ramp up next two channels in this group.

.. figure:: img/exar1.png

Exar configuration

.. figure:: img/exar2.png

Exar power on delays

Maximum power draw for each Mezzaninne
--------------------------------------

+----------------+---------------+
| Power rail[V]  | Current [mA]  |
+----------------+---------------+
| +12            | 200           |
+----------------+---------------+
| -12            | 50            |
+----------------+---------------+
| +6             | 1500          |
+----------------+---------------+
| -6             | 100           |
+----------------+---------------+
| +3.3           | 1000          |
+----------------+---------------+

