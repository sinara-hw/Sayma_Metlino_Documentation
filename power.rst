Power
=====

Power supply
------------

The card can operate as stand alone devide, or plugged into a uTCA crate. While working standalone the power is provided by Molex Connector (39-28-1043) - J4. The pinout is shown below.
When the card is inserted to the crate the power is applied from AMC connector -J2, +12V (8 power lines) and +3.3\_MP (1 line).
In the beginning +3.3\_MP power up the MMC, then +12 is converted to lower voltages, simplified power map is in figure \ref{powermap}. All on board voltages (exept P3V3\_MP) are enabled by MMC. There are two types of power distributors, fixed LDOs and Exar chip - quad channel digital Pulse Width Modulated (PWM) step down (buck) controller. Exar allows for adjust power parameters and for set particular power oorder. Exar firmware monitors current and responds if its too high.  Additionaly all LDOs have connected PowerGood outputs so it is possible to read the proper state of all power busses.

.. todo::
    TBD Can MMC readout Exar debug information?

.. todo::
    TBD voltage noise

.. figure:: img/molex.jpg

Power connector

+-----+-----+
| GND | GND |
+-----+-----+
| +12 | +12 |
+-----+-----+

Maximum board(AMC+RTM module) power consumption estimate to 3A @ 12V.

.. note::
  Please note that power consumption mostly depends from FPGA configuration.

* Input voltage range: 10.8-13.2 [V]
* The board needs active cooling. Approx. 20CFM in 20 C air.

Exar chips are configured via PM\_I2C bus (I2CMUX5) or directly by connecting to W1 (call-out 28) header. For proper configuration **Exar Power Architect** in version **5.2-r1** is needed.

`Exar Power Archtect 5.2-r1 <https://www.exar.com/content/document.ashx?id=21632}{https://www.exar.com/content/document.ashx?id=21632>`_

`Configuration files <https://github.com/m-labs/sinara/tree/master/EXAR\_config}{https://github.com/m-labs/sinara/tree/master/EXAR\_config>`_

`Datasheet <https://www.exar.com/ds/xr77129_1a_120514.pdf>`_

`Quick Start Guide <https://www.exar.com/files/powerxr/PA5-QSG_110_010614.pdf>`_

Actual voltages and current consumption, temperature can be found in Chip Dashboard. There is also oportunity to adjust settings.

.. figure:: img/exarprog.png

Chip Dashboard

Power configuration
-------------------

Power map
^^^^^^^^^

.. figure:: img/pwr.eps

Power map

+---------+-------+--------+
| Voltages and currents    |
+---------+-------+--------+
| P0V9    | 0.9V  | 10A    |
+---------+-------+--------+
| P0V95   | 0.95V | 31mA   |
+---------+-------+--------+
| P1V0    | 1.0V  | 3A     |
+---------+-------+--------+
| P1V2    | 1.2V  | 0.6A   |
+---------+-------+--------+
| P1V5    | 1.5V  | 7.5A   |
+---------+-------+--------+
| P1V8    | 1.8V  | 1.6A   |
+---------+-------+--------+

+-----------------------------------+
| Maximum RTM voltages and currents |
+---------+-------+-----------------+
| P12V0   | 12V   | 3A              |
+---------+-------+-----------------+
| P3V3MP  | 3.3V  | 30mA            |
+---------+-------+-----------------+
	
Exar parameters
^^^^^^^^^^^^^^^

Exar chip(XR77129) has 4 configurable outputs with configirable current limits. Each channel can be confugured individually. It is possible to set voltage, current limit and power sequencing. In Figure \ref{exar1} we can see that main power supply is 12V, Under Voltage Lockout(UVLO) is set to 6V, so below this value chip will shutdown all channels. When the temperature rise under Over Temperature Protection (OTP) 105 degrees, the chip will generate warning event and restart.

All 4 channells can be grouped together and will start-up and shut-down in an user defined sequwnce. Selecting none means channels will not be assigned to any group and therefore, will be controlled independently. Group 0 is controlled by ENABLE or PM\_I2C command. Group 1 can be controlled by GPIO or by PM\_I2C command. By selecting 'Wait PGOOD' next channel will not power up untill current channel reaches the target level. Delay is an additional delay time which postpone after power up one and another channel in group.

In on-going Exar configuration - Figure \ref{exar2}, power sequencing looks like this: After Enable from MMC P1V0 start-up, after it reaches proper value, Exar waits 10ms and turn ramp up another channel in this group - P1V8. In the same order is ramp up last channel in this group. Finally on command 'EN\_PSU\_CH' P3V3 ramp up.

.. figure:: img/exar1.png

Exar configuration

.. figure:: img/exar2.png

Exar power on delays

