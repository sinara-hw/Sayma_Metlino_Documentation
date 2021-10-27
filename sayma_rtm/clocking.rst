Clocking
========

This section describes how and where clock signals are routed.

.. figure:: img/Sayma_RTM_clk.svg

FPGA Oscillators
----------------

* OSC1 - 50 MHz clock source for FPGA resources (not mounted by default)
* OSC2 - 125 MHz clock for MGT

JESD204 synchronization	procedure
---------------------------------

While JESD204B subclass 1 provides "fixed latency" for the data transfer between a DAC and the FPGA, this is fundamentally insufficient for DRTIO. We need more than just fixed latency. A JESD204B link has two device clocks: one for the converter and one for the FPGA. The SYSREF signal is used to designate which cycle of the faster of the two device clocks corresponds to the beginning of a cycle in the slower device clock. The slower device clock and SYSREF have an a priori unknown phase with respect to the RTIO clock.

Timestamping a certain sample to a specific RTIO cycle requires two things in addition to JESD204B subclass 1 deterministic latency:

* Reproducible alignment of the sample clock with the RTIO clock. This is guaranteed by fixed latencies in the DRTIO branch of the clocking (master oscillator -> Metlino -> AMC backplane DRTIO link -> Sayma AMC). This also requires the backplane clock and the sample clock to be integer multiples of the RTIO clock.
* Reproducible alignment of SYSREF and the slower FPGA device clock to the RTIO clock. This is done actively.

The FPGA shall align SYSREF with designated RTIO clock edges. The alignment should be better than a DAC clock cycle and reproducible across reboots.

The FPGA first roughly aligns SYSREF within one cycle before a desired RTIO clock edge by asserting the synchronization signal of the clock chip, which resets its dividers. This alignment is optional and may have an uncertainty of several DAC clock cycles. It is only used to decrease the required scan range of the delay elements used in the next steps.

The FPGA then analyzes SYSREF by repeatedly sampling it with the RTIO clock while scanning a calibrated I/O input delay. This measures the SYSREF phase with a high precision.

The delay scan mechanism is limited by the resolution and stability of the scan element. The resolution must be significantly smaller than a DAC clock period. There are three delay elements available to perform the scan:

* IDELAYE3 in the FPGA. Uncertainty about PVT effects.
* Digital delay in the clock distribution chip. Infinite delay, low noise.
* Analog delay in the clock distribution chip (HMC704X only, not AD9516-1). Very fine and well calibrated, but too noisy to be used on a sample clock.

We plan to use the latter two elements for the scan.

The FPGA then rounds the phase to an integer multiple of sample clock cycles using previously stored fractional delay data (delay <- round(measured - fractional)) and stores the new fractional delay (fractional <- measured - delay). It now programs the digital phase shifters of the slower clocks (FPGA deviceclock and SYSREF) with the negative of the rounded delay value.

Sayma RTM clock chip connections
--------------------------------

The HMC7044 has 10 outputs connected. They are used for:

* DAC0 device clock
* DAC0 SYSREF
* DAC1 device clock
* DAC1 SYSREF
* FPGA SYSREF with fine delay
* FPGA MGT reference clock for DAC x2
* additional outputs to FPGA, usable e.g. if we have problems with the recovered RTIO clock.

Clock constraints
-----------------

* t\ :sub:`RTIO` = n * 1ns

    * period of the coarse RTIO clock
    * n integer to avoid rounding errors and beating between RTIO clock and user habit
    * n not necessarily a power of two
    * the same throughout the ARTIQ tree to avoid beating of channels

* t\ :sub:`DRTIO\_link` = n * 10 * t\ :sub:`RTIO` with n being 1, 2, 4, 8

	* line period of the DRTIO link
	* due to 8b10b and parallel bus width
	* n not a power of two could work but looks impractical.
	* does not need to be the same n for each link
	* AMC backplane links can probably not to 10 GHz line rate but 5 GHz, fibers (SFP+) can

* t\ :sub:`SAWG\_DATA` = t\ :sub:`RTIO`/{value from table below}


+---------------------------------+--------------+
| f\ :sub:`DAC`/f\ :sub:`SAWG`    | {1, 2, 4, 8} |
+---------------------------------+--------------+
| f\ :sub:`SAWG`/f\ :sub:`RTIO`   | {1, 2, 4, 8} |
+---------------------------------+--------------+
| f\ :sub:`RTIO`/f\ :sub:`DRTIO`  | {10, 20, 40} |
+---------------------------------+--------------+
| f\ :sub:`JESD_P`/f\ :sub:`RTIO` | {1, 2}       |
+---------------------------------+--------------+
| f\ :sub:`JESD`/f\ :sub:`JESD_P` | {40}         |
+---------------------------------+--------------+


+--------+----------------+-----------------+--------------------+-----------------+-----------------+------------------+
| (GHz)  | f\ :sub:`DAC`  | f\ :sub:`SAWG`  | f\ :sub:`JESD\_P`  | f\ :sub:`JESD`  | f\ :sub:`RTIO`  | f\ :sub:`DRTIO`  |
+--------+----------------+-----------------+--------------------+-----------------+-----------------+------------------+
| A      | 2.4            | 0.6             | 0.15               | 6               | 0.15            | 3                |
+--------+----------------+-----------------+--------------------+-----------------+-----------------+------------------+
| B      | 2              | 1               | 0.25               | 10              | 0.125           | 5                |
+--------+----------------+-----------------+--------------------+-----------------+-----------------+------------------+
| C      | 0.3            | 0.3             | 0.15               | 6               | 0.15            | 3                |
+--------+----------------+-----------------+--------------------+-----------------+-----------------+------------------+

