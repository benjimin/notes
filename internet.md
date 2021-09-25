Internet
========


Physical layer
--------------

All computer networking relies utilises electromagnetic waves. Even in wired ethernet, there are multiple successive signals in flight along any segment. Some methods require collision-detection (such as old coaxial cable 10base2 10Mbps ethernet, and wireless 

Current examples:
- Electrical conductor (i.e. copper)
  - Gigabit ethernet (1Gbps 1000base-T): uses four parallel pairs of conductors. Each pair is twisted individually, to balance interference (cancelling noise in the pairwise-difference signals). Each individual pair transmits carrier voltage-pulses overlaid with five-level amplitude modulation (but one of these five is reserved for error detection, so each symbol ideally represents 2 bits) at 125 million baud (symbols per second). The communication occurs full-duplex. This means there is a different 4 symbols (together representing a byte i.e. 8 bits) in flight for every couple metres of the cable, in each direction.
  - ADSL
- Fibre optic
  - 100 Gigabit ethernet (e.g. 100Gbase-LR1). Uses near-infrared. Typically PAM4 encoding of a single laser wavelength.
  - Undersea backbone cables, ~100Tbps. Uses multiple fibres, wavelength division multiplexing, and light amplification.
- Open air
  - Wifi (e.g. 54Mbps 802.11g and ~0.5 GBps 802.11ac). 
  - 5G.
