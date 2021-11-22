Sayma
=====

Sayma is a hardware that supports M-Lab's Smart Arbitrary Waveform Generator (`SAWG <http://m-labs.hk/artiq/manual-master/core_drivers_reference.html?highlight=sawg#module-artiq.coredevice.sawg>`_) gateware.
It provides 8 channels of 1.2 GSPS 16-bit DACs (2.4 GHz DAC clock). It
consists of an AMC, providing the high-speed digital logic and memory, and an RTM,
holding the data converters and low-noise analog components.

The design files are located in
`Sayma AMC <https://github.com/sinara-hw/Sayma_AMC/>`_
and
`Sayma RTM <https://github.com/sinara-hw/Sayma_RTM/>`_
repositories. PDF schematics are located in release section of each repository.
The PCBs are double width, mid height AMC module.

Features
--------

* May be used in a uTCA rack or stand-alone operation with fibre-based DRTIO link
* Analog input and output front-ends provided by plug-in
  :ref:`afe` modules (eg. BaseMod) for maximum
  flexibility.
* High channel density - 8 output channels per board, up to 12 boards per crate.
* FMC LPC slot

Schematic / Layout Viewer
-------------------------

Hardware was designed under Altium Designer CAD tool.
Project resources are in separate repositories.
Read-only access to PCB schematics and layout designs is possible using free tool called 
`Altium Designer viewer <http://www.altium.com/altium-designer-viewer>`_.

Sayma AMC
---------

.. toctree::
    :maxdepth: 2

    sayma_amc/description
    sayma_amc/view
    sayma_amc/routing
    sayma_amc/clocking
    sayma_amc/i2c
    sayma_amc/panel
    sayma_amc/fmc
    sayma_amc/usb-uart
    sayma_amc/jtag
    sayma_amc/power
    sayma_amc/mmc
    sayma_amc/housekeeping
    sayma_amc/signals
    
Sayma RTM
---------

.. toctree::
    :maxdepth: 2
    
    sayma_rtm/description
    sayma_rtm/view
    sayma_rtm/routing
    sayma_rtm/clocking
    sayma_rtm/i2c
    sayma_rtm/panel
    sayma_rtm/AFEmezzanine
    sayma_rtm/housekeeping
    sayma_rtm/power
    sayma_rtm/signals
    sayma_rtm/technical
