# IPv4 Header

---

## Purpose of the IPv4 Header

IPv4 operates at **Layer 3 (Network Layer)**.

* Responsible for moving data between **different networks**
* This process is called **routing**

The IPv4 header provides routers with:

* Source IP address
* Destination IP address
* Instructions on how to handle the packet

---

## Protocol Data Units (PDUs)

Encapsulation process:

```text
Data → Segment → Packet → Frame
```

* Layer 4 → Segment
* Layer 3 → Packet
* Layer 2 → Frame

Layer 3 PDU = **Packet**

---

## Key Concept

The IPv4 header is not meant to be memorized fully.
Focus on:

* Purpose of key fields
* What routers actually use
* How fields affect behavior

---

## IPv4 Header Fields (Important Only)

---

### Version

* 4 bits
* Identifies IP version

```text
IPv4 = 4
IPv6 = 6
```

---

### IHL (Internet Header Length)

* 4 bits
* Length of header in **4-byte increments**

```text
Minimum = 5 → 20 bytes
Maximum = 15 → 60 bytes
```

---

### DSCP

* 6 bits
* Used for **QoS (traffic prioritization)**

---

### ECN

* 2 bits
* Indicates congestion without dropping packets

---

### Total Length

* 16 bits
* Total packet size (header + data)

```text
Minimum = 20 bytes
Maximum = 65,535 bytes
```

---

### Identification

* 16 bits
* Used for **fragmentation tracking**

---

### Flags

* 3 bits

Important flags:

* DF (Don’t Fragment)
* MF (More Fragments)

Rules:

* DF = no fragmentation allowed
* MF = more fragments coming
* Last fragment → MF = 0

---

### Fragment Offset

* 13 bits
* Indicates position of fragment in original packet

---

### TTL (Time To Live)

* 8 bits
* Prevents infinite routing loops

Behavior:

* Each router decrements TTL by 1
* If TTL = 0 → packet is dropped

Common values:

```text
64 (common default)
255 (maximum)
```

---

### Protocol

* 8 bits
* Identifies Layer 4 protocol

Important values:

```text
1   → ICMP
6   → TCP
17  → UDP
89  → OSPF
```

---

### Header Checksum

* 16 bits
* Verifies **header only**

Key behavior:

* Recalculated at every router
* If invalid → packet is dropped

---

### Source IP Address

* 32 bits
* Sender’s IP address

---

### Destination IP Address

* 32 bits
* Receiver’s IP address

Routers use this field to make forwarding decisions.

---

### Options (Rare)

* Optional field
* Rarely used
* Present only if header > 20 bytes

---

## Practical Behavior

### TTL in Action

* Packet moves across routers
* TTL decreases at each hop
* Prevents infinite loops

---

### Fragmentation Concept

Occurs when:

* Packet size > MTU
* Network cannot carry full packet

If DF is set:

* Packet is dropped instead of fragmented

---

## Relevant Command (Testing Behavior)

```bash
ping <ip-address>
```

Some platforms support:

```bash
ping <ip-address> size <bytes>
```

Used to test:

* MTU limits
* Fragmentation behavior

---

## Key Takeaways

* IPv4 header controls how packets move across networks 
* Routers rely heavily on:

  * Destination IP
  * TTL
  * Protocol
* TTL prevents loops
* Fragmentation occurs when packet exceeds MTU
* Header checksum validates header only

---

## What to Remember

* TTL purpose
* Protocol numbers (1, 6, 17, 89)
* Fragmentation concept
* Header checksum = header only

---

## Self-Check

1. What happens when TTL reaches 0?
   → Packet is dropped

2. What does the protocol field identify?
   → Layer 4 protocol

3. What happens if DF is set and packet is too large?
   → Packet is dropped

4. What field do routers use to forward packets?
   → Destination IP address
