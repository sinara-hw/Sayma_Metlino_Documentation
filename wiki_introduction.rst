Introduction to Sinara
======================

Sinara is an open-source hardware ecosystem originally designed for use
in quantum physics experiments running the
`ARTIQ <https://m-labs.hk/artiq/>`_ control software. It is licensed
under CERN OHL v1.2.

Control electronics used in many trapped-ion and other quantum physics
experiments suffers from a number of problems. In general, an ad-hoc
solution is hastily put together in-house without enough consideration
about good design, reproducibility, testing and documentation. This
makes those systems unreliable, fragile, and difficult to use and
maintain. It also duplicates work in different laboratories. In
addition, the performance and features of the existing systems
(e.g. regarding pulse shaping abilities) is becoming insufficient for
some experiments.

To alleviate those problems, Sinara aims to be:

* high-quality
* simple to use and "turn-key"
* reproducible and open
* flexible and modular
* well tested
* well supported by the ARTIQ control software

.. To see how Sinara can be used in your labs, take a look at our \href{CaseStudies}{case studies} showing Sinara in Action.

Sinara is currently developed by a collaboration including
M-Labs, Warsaw University of Technology (WUT), US Army Research
Laboratory (ARL), the University of Oxford, the University of Maryland
and NIST. The majority of the hardware was designed by WUT. The work was
funded by ARL, Duke University, the University of Oxford, and the
University of Freiburg.

Currently, most of this hardware is at the production stage. Information about the status of the various hardware projects making up Sinara can be found `here <https://github.com/sinara-hw/meta/wiki/Status>`__. Advice about purchasing Sinara hardware can be found `here <https://github.com/sinara-hw/meta/wiki/Purchasing>`__.

Following the ARTIQ model, an experiment consists of a core device
(master) -- typically either a `Metlino <https://github.com/sinara-hw/Metlino/wiki>`_ or `Kasli <https://github.com/sinara-hw/Kasli/wiki>`_ --
controlling multiple slave devices in real time using ARTIQ's
`distributed real-time
IO <https://github.com/m-labs/artiq/wiki/DRTIO>`_ (DRTIO) protocol. DRTIO provides both gigabit communication links
and time distribution over copper cable or optical fibres. It
synchronises all device clocks, ensuring they have deterministic phase
relationships, and enables nanosecond timing resolution for input and
output events across all devices in the experiment.

.. More detailed information about communication between devices and time distribution inside Sinara can be found \href{SinaraClocking}{here}.

Sinara uses two main form factors for hardware requiring real-time
control: microTCA (uTCA) and Eurocard Extension Modules (EEM). Non
real-time hardware is typically connected to the host PC using Ethernet.

MicroTCA (uTCA) is Sinara's preferred form factor for high performance
hardware with high-speed data converters requiring deterministic phase
control, such as the Sayma Smart Arbitrary Waveform
Generator (SAWG).

EEMs provide a lower cost, simpler platform than uTCA for hardware that
requires real-time control, but not bandwidth or complexity of uTCA
hardware.

Extension modules connect to a carrier, such as `Kasli <https://github.com/sinara-hw/Kasli/wiki>`_ or the
`VHDCI carrier <https://github.com/sinara-hw/VHDCI_Carrier/wiki>`_, which provides power and DRTIO. They
are designed to be mounted either in stand-alone enclosures, or in a
rack with a carrier, and connect to the carrier via ribbon cable. More
details about the extension module standard can be found
`here <https://github.com/sinara-hw/meta/wiki/EEM>`__.

uTCA hardware interfaces with the extension modules either directly,
using a `VHDCI carrier <https://github.com/sinara-hw/VHDCI_Carrier/wiki>`_, or indirectly, using a
`Kasli <https://github.com/sinara-hw/Kasli/wiki>`_ DRTIO slave.
