Internet
========


Physical layer
--------------

All computer networking is founded on electromagnetic wave propagation. Some methods necessitate collision-detection (such as old coaxial cable 10base2 10Mbps ethernet, and wireless methods).

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

Networks
--------

### MAC addressing

Network links can be bridged at a logical level, only retransmitting well-formed dataframes. This is performed in star topologies by a switch.

Each node is expected to have a unique MAC (media access control) address. The switch tries to remember which MAC addresses are connected to each interface, to forward dataframes to the correct link. (Otherwise the dataframe will need be broadcast over all interfaces.) A switch may associate multiple MAC addresses to the same interface, for example if it leads to another switch (or similarly, to a host with multiple virtual machines). 

High speed switches are likely to use specialised circuitry. (100Gbps implies receiving 64bits from each port in the period that a conventional 64-bit CPU core executes 1-2 instructions.)

### IP routing

The internet protocol enables separate switched networks to be connected in a hierarchical manner. IPv4 involves 32bit addresses which are divided into a prefix that is particular to the local switched network, and an extension that is peculiar to individual hosts within that subnet. Each host must be configured with its address, the prefix length (nowadays expressed with CIDR notation instead of netmask), and the address of a default gateway (a host on the local network that can relay message packets for addresses outside of the subnet). 

The prefix length permits recognising whether another IP address is on the same subnet. This determines whether a packet should be directed to the MAC address of the gateway or of the recipient directly. A sender will check its own cache for the MAC address associated with a given local IP address, and otherwise needs to broadcast (using Address Resolution Protocol) to find which MAC address answers for that IP address. (ARP can be vulnerable to an interception attack, where a third MAC address self-identifies to a target as the gateway IP, and to the actual gateway as that host IP.)

The most common protocols used over IP are either UDP or TCP. Each of these has its own notion of numerical ports, to separately address multiple services on the same host. The user datagram protocol is just a lightweight wrapper for IP datagrams. Transmission control protocol is more sophisticated, establishing connections with reliable, ordered data streams (through handshaking, retransmissions, error detection, etc).


Security
--------

### Cryptography

#### Hashing

A hash is a one-way condensation of an arbitrary length input into a short digest. (Similar to a checksum it is intended to be sensitive to plausible changes of the input, but a cryptographic hash must also make it difficult to engineer inputs that have collisions in output digest value.) Hash representations are commonly used as indexes by data repositories (git, docker, etc) where they serve a secondary function for integrity checking of the content. Authentication also tends to store hashed representations (of passwords each combined with a different random salt) so that any leaked hashes must each be cracked independently to expose the password.

Git uses SHA1, and is ostensibly planning to migrate to SHA256. (Collision attacks have been demonstrated for SHA1, and likewise for the related MD5.) Ubuntu Linux presently defaults to SHA512 for system passwords. These algorithms center around sequences of bitwise operations (such as xor gates and circular shifts) that are iterated dozens of times to scramble the input. The plaintext is divided into fixed length chunks, and scrambling for each successive chunk is seeded using the output from the preceding chunk.

#### Symmetric encryption

Symmetric encryption is used to password-protect files (such as private keys). 

#### Asymmetric encryption

Ed25519 appears to be the current recommended practice (uses elliptic curve). The key is quite short (only 256 bits for the public key).

If instead using RSA, should use 4096 bits.

#### Signatures

If two parties have a shared secret (a random value), then as a signature they can use the hash of the combination of the message and the key. To verify the signature, the other party generates their own signature for the message and then checks that both signatures match. This is the principle of the HMAC-SHA256 signing algorithm used for most JWTs (e.g. for OIDC) and is also the basis of the AWS SigV4 protocol (for using AWS credentials to authorise API requests).

#### Cracking

The critical measure is how much money does it take to crack a password or key by brute force. Brute forcing is trivially parallelisable, compute time is a fungible commodity, so the security reduces to a question of resources not time. It is straightforward to benchmark the rate of attempts and extrapolate e.g. to various password complexities. An upper bound on the warranted complexity can be obtained from the value of the resource being protected (or the market value of the owning organisation), or the resources of the largest potential competition (e.g., the revenue of a foreign government over a plausible span of time), or an estimate of the cost to go around the cryptography (e.g., obtaining credentials via espionage or coercion).

The caveat is that quantum technological advancements (or to a lesser extent, mathematical breakthroughs) are anticipated to break all cryptography, except one-time pads, in the foreseeable future, dramatically revising these prices downward thereafter.

### Certificates

Normally expressed in a canonical format.

Typically, the entire trust chain is sent at once, so that the client can immediately verify the claims.

### TLS (formerly SSL)

Transport Layer Security 1.3 (successor of Secure Sockets Layer) underpins most secure communications, such as HTTPS.

RFC8446

The client sends a random nonce. Hash pairs, key shares, key (by label)..

ECH (Encrypted Client Hello) and SNI (Server Name Indication)


### SSH keys



### Authentication

The JSON Web Token is a standard for encoding a cryptographically verified claim. It has three parts: a header (a JSON structure that confirms it is a JWT and specifies the crypto algorithm), the payload (a JSON structure of claims, typically including: issuer, subject, audience, and expiry), and a signature (a hash of the rest of the token, generated using either a shared secret or asymmetric key pair). The parts are base64 url encoded and concatenated (with periods as separators). 

OAuth2 is an authorisation standard. The common flow (for applications that have a backend servers) is that the application server redirects the client browser to the authorisation service. That service authenticates the user (say by password) and redirects the client back to the application server along with an authorisation code. The application server then contacts the authentication service directly, exchanging the code and application key for tokens (to identify the user and to grant the application server access to resources on the user's behalf). 

OpenID Connect is the authentication standard built on OAuth2. It defines an ID token, as well as a user-info service (which uses the access token for authorisation). 
