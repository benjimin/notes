Internet
========


Physical layer
--------------

All computer networking relies utilises on electromagnetic wave propagation. Some methods require collision-detection (such as old coaxial cable 10base2 10Mbps ethernet, and wireless methods).

Current examples:
- Electrical conductor (i.e. copper)
  - Gigabit ethernet (1Gbps 1000base-T): uses four parallel pairs of conductors. Each pair is twisted individually, to balance interference (cancelling noise in the pairwise-difference signals). Each individual pair transmits voltage changes (i.e. five-level voltage modulation, but one of these five is reserved for error detection, so each symbol usually encodes 2 bits) at 125 million baud (symbols per second). The communication occurs full-duplex. This means there is a different set of four symbols (together representing a byte i.e. 8 bits) in flight, in each direction, for every couple metres of the cable.
  - ADSL
- Fibre optic
  - 100 Gigabit ethernet (e.g. 100Gbase-LR1). Uses near-infrared. Typically PAM4 encoding of a single laser wavelength. Generally uses single-mode fiber (as dispersion between spatial modes of wave propagation would restrict signal bandwidth) and laser illumination (as LED results in some chromatic dispersion). 
  - Undersea backbone cables, ~100Tbps. Uses multiple parallel fibres, wavelength division multiplexing, and optical amplification (at regular intervals along the cable).
- Open air
  - Wifi (e.g. 54Mbps 802.11g and ~0.5 GBps 802.11ac). 
  - 5G.

A repeater is generally used to transfer a physical signal between consecutive physical links, with amplification and typically re-modulation to counter not only attenuation but also dispersion and interference. (A hub was a repeater in star topology; by re-broadcasting multiple links it necessitates collision detection, limiting the channels to half duplex.)

MAC addressing
--------------

Network links can be bridged at a logical level, only retransmitting well-formed dataframes. This is performed in star topologies by a switch.

Each node is expected to have a unique MAC (media access control) address. The switch tries to remember which MAC addresses are connected to each interface, to forward dataframes to the correct link. (Otherwise the dataframe will need be broadcast over all interfaces.) A switch may associate multiple MAC addresses to a single interface, for example if it connects to another switch (or similarly, to a host with multiple virtual machines). 

High speed switches are likely to use specialised circuitry. (100Gbps implies receiving 64bits from each port in the period that a conventional 64-bit CPU core executes 1-2 instructions.)

IP routing
----------

The internet protocol enables separate switched networks to be connected in a hierarchical manner. IPv4 involves 32bit addresses which are divided into a prefix that is particular to the local switched network, and an extension that is peculiar to individual hosts within that subnet. Each host must be configured with its address, the prefix length (nowadays expressed with CIDR notation instead of netmask), and the address of a default gateway (a host on the local network that can relay message packets for addresses outside of the subnet). 

The prefix length permits recognising whether another IP address is on the same subnet. This determines whether a packet should be directed to the MAC address of the gateway or of the recipient directly. A sender will check its own cache for the MAC address associated with a given local IP address, and otherwise needs to broadcast (using Address Resolution Protocol) to find which MAC address answers for that IP address. (ARP can be vulnerable to an interception attack, where a third MAC address self-identifies to a target as the gateway IP, and to the actual gateway as that host IP.)


Security
--------

### Hashing

Git uses SHA1, and is planning to migrate to SHA256. (Collision attacks have been demonstrated for SHA1, likewise MD5.)

Ubuntu Linux presently defaults to SHA512 for system passwords.

### Symmetric encryption

Symmetric encryption is used to password-protect files (such as private keys). 

### Asymmetric encryption

Ed25519 appears to be the current recommended practice (uses elliptic curve). The key is quite short (only 256 bits for the public key).

If instead using RSA, should use 4096 bits.

### TLS (formerly SSL)

Transport Layer Security (successor of Secure Sockets Layer).

RFC8446

The client sends a random nonce. Hash pairs, key shares, key (by label)..

Certificates

ECH (Encrypted Client Hello) and SNI (Server Name Indication)


### SSH keys

### Certificates

