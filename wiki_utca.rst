Introduction to Micro TCA
=========================

uTCA is a modular, open standard originally developed by the
telecommunications industry. It allows a single rack master -- the Micro
TCA Carrier Hub (MCH) -- to control multiple slave boards, known as
Advanced Mezzanine Cards (AMCs) via a high-speed digital backplane. uTCA
chassis and backplanes are available commercially of the shelf (COTS).

We make use one of the most recent extension to the uTCA standard, uTCA.4.
Originating in the high-energy and particle physics (HEPP) community,
uTCA.4 introduces rear-transition modules (RTMs). Each RTM connects to an AMC
(one RTM per AMC). Typically, the AMCs hold FPGAs and other high-speed
digital hardware, communicating with the MCH via gigabit serial links
over the AMC backplane. The RTMs hold data converters and other
low-noise analog components, controlled by the corresponding AMC. RTM cards 
are screened from AMCs to minimise interference from the high-speed digital logic.

.. figure:: img/MTCA_Front1.jpg

Micro TCA chassis with 3 Sayma AMC modules inserted

.. figure:: img/MTCA_Back.jpg

Micro TCA chassis with 4 RTM modules inserted. One of them has 4 BaseMod AFE mezzanines installed.

uTCA parts and suppliers
------------------------
These are parts, that were validated for use with Sayma AMC+RTM and with Metlino.

* NAT AC 600D, qty 1
* NAT MCH-Basic v3.5, full-size, double width front panel, qty 1
* NAT Native-R5, qty 1

Glossary
--------

* **AMC Module or Module**: An AMC Module is a mezzanine or modular add-on card that extends the functionality of a Carrier Board. The term is also used to generically refer to the different varieties of Multi-Width and Multi-Height Modules.
* **Fat Pipes**: Ports 4 though 11 of the AMC Connector constitute the Fat Pipes Region. This Region of Ports is intended for the assignment of multiple Lane interfaces, also called “fat pipes”. Fat Pipe 1 [Ports 4-7], Fat Pipe [Ports 8-11].
* **FMC**: FPGA Mezzanine Card
* **Hot Swap**: To remove a component (e.g., an AMC Module) from a system (e.g., an AMC Carrier AdvancedTCA Board) and plug in a new one while the power is still on and the system is still operating.
* **Management Power or MP** The 3.3V power for a Module's Management function, individually provided to each Slot by the Carrier
* **IPMB**: Intelligent Platform Management Bus. The lowest level hardware management bus as described in the Intelligent Platform Management Bus Communications Protocol Specification.
* **MGT**: Multi-Gigabit Transceiver
* **MMC**: Module Management Controller. The MMC is the required intelligent controller that manages the Module and is interfaced to the Carrier via IPMB-Local
* **RTM**: Rear Transition Module
