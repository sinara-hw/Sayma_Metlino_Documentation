Routing
=======

This section contain block schematics of Sayma AMC board.

.. _amc_io_connections:

I/O connections
---------------

General block schematic below shows simplified connections between I/O and components. Some unused connectors were left out to improve readability of this schematic. Front panel connectors (green) and back connectors (blue) are arranged in the same order as in the PCB.

.. figure:: img/Sayma_AMC_general.svg

    General block schematic

.. _transceiver_connections:

Transceiver connections
-----------------------

Block schematic below shows connections to Multi-Gigabit Transceivers (MGT) of the FPGA. Switches symbolize connections that can be altered by placement of capacitors. Default connection is symbolized by switch position.

.. figure:: img/Sayma_AMC_MGT.svg

    MGT schematic with optional connections

Table below outlines connections to MGT ports of the FPGA. If there are two possibilities then the default one is listed first. RTM_MGT connections are TX only by default.

+----------------------+---------------------+---------------------------------------------+
|   Transceiver        |   Direction         |   Routed to                                 |
+======================+=====================+=============================================+
| 0\_224               | RX & TX             | SFP0                                        |
+----------------------+---------------------+---------------------------------------------+
| 1\_224               | RX & TX             | PORT 0 or SFP1                              |
+----------------------+---------------------+---------------------------------------------+
| 2\_224               | RX & TX             | FAT PIPE 1 PORT 4 & SATA J16                |
+----------------------+---------------------+---------------------------------------------+
| 3\_224               | RX & TX             | RTM\_MGT or (FAT PIPE 1 PORT 5 & SATA J17)  |
+----------------------+---------------------+---------------------------------------------+
| 0\_225               | TX (optionally RX)  | RTM\_MGT                                    |
+----------------------+---------------------+---------------------------------------------+
| 1\_225               | TX (optionally RX)  | RTM\_MGT                                    |
+----------------------+---------------------+---------------------------------------------+
| 2\_225               | TX (optionally RX)  | RTM\_MGT                                    |
+----------------------+---------------------+---------------------------------------------+
| 3\_225               | TX (optionally RX)  | RTM\_MGT                                    |
+----------------------+---------------------+---------------------------------------------+
| 0\_226               | TX (optionally RX)  | RTM\_MGT                                    |
+----------------------+---------------------+---------------------------------------------+
| 1\_226               | TX (optionally RX)  | RTM\_MGT                                    |
+----------------------+---------------------+---------------------------------------------+
| 2\_226               | TX (optionally RX)  | RTM\_MGT                                    |
+----------------------+---------------------+---------------------------------------------+
| 3\_226               | TX (optionally RX)  | RTM\_MGT                                    |
+----------------------+---------------------+---------------------------------------------+
| 0\_227               | TX (optionally RX)  | RTM\_MGT                                    |
+----------------------+---------------------+---------------------------------------------+
| 1\_227               | TX (optionally RX)  | RTM\_MGT                                    |
+----------------------+---------------------+---------------------------------------------+
| 2\_227               | TX (optionally RX)  | RTM\_MGT                                    |
+----------------------+---------------------+---------------------------------------------+
| 3\_227               | TX (optionally RX)  | RTM\_MGT                                    |
+----------------------+---------------------+---------------------------------------------+
| 0\_228               | TX (optionally RX)  | RTM\_MGT                                    |
+----------------------+---------------------+---------------------------------------------+
| 1\_228               | TX (optionally RX)  | RTM\_MGT                                    |
+----------------------+---------------------+---------------------------------------------+
| 2\_228               | TX (optionally RX)  | RTM\_MGT                                    |
+----------------------+---------------------+---------------------------------------------+
| 3\_228               | TX (optionally RX)  | RTM\_MGT                                    |
+----------------------+---------------------+---------------------------------------------+

.. note:: Only port 0 and port 4 are connected to transceivers. Sayma AMC will receive Ethernet only from left (main) MCH slot (though Ethernet is not used by default in Artiq on Sayma). Sayma AMC will only connect to Metlino in left (main) MCH slot.
