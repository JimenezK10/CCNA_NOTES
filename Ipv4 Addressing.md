# IPv4 Addressing

---

## Network Layer (Layer 3)

Layer 3 is the **Network layer**.

* Routers operate at Layer 3
* Provides:

  * Connectivity between **different networks**
  * **Logical addressing (IP addresses)**
  * **Path selection**

Switches operate within a LAN.
Routers connect multiple networks.

---

## Routing Fundamentals

* Devices connected through switches are in the **same IP network**
* Switches **do not separate networks**
* Routers **separate networks**

Example:

```text
192.168.1.0/24
```

* `/24` = prefix length
* First 24 bits = **network portion**
* Last 8 bits = **host portion**

---

## Router Interfaces and IP Addressing

* A router requires:

  * One IP address per connected network
  * Each IP is assigned to an **interface**

Example:

```text
Router
 ├─ G0/0 → 192.168.1.254/24
 └─ G0/1 → 10.1.1.254/24
```

Each interface belongs to a **different network**.

### Key Concept

```text
Each router interface = one network
```

---

## Switch Behavior in This Context

Switches operate at Layer 2 and do not require IP addresses to forward traffic.

* Switches use **MAC addresses**, not IP addresses
* Frames are forwarded based on the **MAC address table**
* No routing decisions are made

In this lab:

* Switches did not need IP addresses because:

  * They were only forwarding traffic within a LAN
  * No remote management (SSH/Telnet) was required
  * No Layer 3 functionality was being used

### Important Distinction

* Router interface IP → used as **default gateway**
* Switch → forwards frames without needing an IP

---

## Broadcast Domain Rule

* Layer 2 broadcast traffic does **not cross routers**
* Routers act as **broadcast boundaries**

If a broadcast frame reaches a router:

* It is **not forwarded**
* It stops at that interface

---

## IPv4 Address Basics

* IPv4 = **32 bits (4 bytes)**
* Written in **dotted decimal**

Example:

```text
192.168.1.254
```

Binary:

```text
192 → 11000000
168 → 10101000
1   → 00000001
254 → 11111110
```

* Each 8-bit segment = **octet**
* Total = **4 octets**

---

## Subnet Masks and Prefix Length

* Prefix length defines:

  * Network bits
  * Host bits

Example:

```text
154.78.111.32/16
```

* Network portion: `154.78`
* Host portion: `111.32`

---

## Binary Values (Subnet Masks)

| Binary   | Decimal |
| -------- | ------- |
| 10000000 | 128     |
| 11000000 | 192     |
| 11100000 | 224     |
| 11110000 | 240     |
| 11111000 | 248     |
| 11111100 | 252     |
| 11111110 | 254     |
| 11111111 | 255     |

* Subnet masks use **continuous 1s followed by 0s**

---

## IPv4 Address Classes

| Class | First Octet | Default Prefix |
| ----- | ----------- | -------------- |
| A     | 1–126       | /8             |
| B     | 128–191     | /16            |
| C     | 192–223     | /24            |
| D     | 224–239     | Multicast      |
| E     | 240–255     | Reserved       |

Focus:

* Class A, B, C

---

## Loopback Addresses

```text
127.0.0.0 – 127.255.255.255
```

* Used to test the **local system**

```bash
ping 127.0.0.1
```

* Traffic never leaves the device

---

## Network and Broadcast Addresses

Each subnet contains:

* **Network address** (all host bits = 0)
* **Broadcast address** (all host bits = 1)

Example:

```text
192.168.1.0/24     → Network address
192.168.1.255      → Broadcast address
```

Usable range:

```text
192.168.1.1 – 192.168.1.254
```

---

## Broadcast Behavior

Layer 3 broadcast:

```text
192.168.1.255
```

Layer 2 broadcast:

```text
ffff.ffff.ffff
```

---

## Netmask Notation

| Prefix | Netmask       |
| ------ | ------------- |
| /8     | 255.0.0.0     |
| /16    | 255.255.0.0   |
| /24    | 255.255.255.0 |

---

## Verification Commands

```bash
ping <ip-address>
```

```bash
ping 127.0.0.1
```

---

## Key Takeaways

* Routers connect different networks and require IPs per interface
* Switches forward traffic using MAC addresses and do not require IPs
* Default gateway = router interface IP
* Subnet masks define network vs host portions
* Broadcast traffic does not cross routers

---

## Self-Check

1. Why doesn’t a switch need an IP to forward traffic?
   → Because it operates at Layer 2 using MAC addresses

2. What is the role of a router interface?
   → Connects a network and provides a gateway

3. What happens without a default gateway?
   → Traffic cannot leave the local network

