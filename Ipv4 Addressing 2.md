# IPv4 Addressing — Part 2

---

## Classful Basics and Host Calculation

* Class A usable range:

```text
1–126
```

* `127` is reserved for loopback

### Host Formula

```text
2^n − 2
```

* Subtract:

  * Network address
  * Broadcast address

### Example (/24)

* Total addresses: 256
* Usable hosts:

```text
256 − 2 = 254
```

---

## First and Last Usable Addresses

General rules:

* Network address = all host bits = 0
* Broadcast address = all host bits = 1
* First usable = network + 1
* Last usable = broadcast − 1

---

### Example (/8)

```text
10.0.0.0/8        → Network
10.0.0.1          → First usable
10.255.255.255    → Broadcast
10.255.255.254    → Last usable
```

---

### Example (/16)

```text
172.16.0.0/16     → Network
172.16.0.1        → First usable
172.16.255.255    → Broadcast
172.16.255.254    → Last usable
```

---

## Binary Intuition

* First usable → last host bit flips from 0 → 1
* Last usable → last host bit flips from 1 → 0

---

## Router Interface Status

### Command

```bash
show ip interface brief
```

### Output Fields

* Interface → physical/logical interface
* IP-Address → assigned IP
* Method → how IP was assigned
* Status → Layer 1 (physical)
* Protocol → Layer 2 (data link)

### Key Rule

```text
If Layer 1 is down → Layer 2 must be down
```

---

## Default Behavior

* Router interfaces → shutdown by default
* Switch interfaces → enabled by default

---

## Interface Configuration Mode

```bash
configure terminal
interface gigabitEthernet0/0
```

Shorthand:

```bash
int g0/0
```

---

## Assigning an IP Address

```bash
ip address <ip> <subnet-mask>
```

Example:

```bash
ip address 10.255.255.254 255.0.0.0
```

* Subnet mask is required

---

## Enabling an Interface

```bash
no shutdown
```

Shorthand:

```bash
no shut
```

### System Messages

* `%LINK-5-CHANGED` → Layer 1
* `%LINEPROTO-5-UPDOWN` → Layer 2

---

## Verifying Interfaces

From config mode:

```bash
do show ip interface brief
```

Shorthand:

```bash
do sh ip int br
```

Healthy interface:

```text
Status = up
Protocol = up
```

---

## Default Gateway Concept

* Router interface IP is used as the **default gateway**
* Common choices:

  * First usable address
  * Last usable address

Consistency matters more than choice

---

## Multiple Interface Configuration Example

```bash
int g0/1
ip address 192.168.0.254 255.255.255.0
no shut

int g0/2
ip address 172.16.255.254 255.255.0.0
no shut
```

---

## Subnet Mask Quick Reference

| Prefix | Subnet Mask   |
| ------ | ------------- |
| /8     | 255.0.0.0     |
| /16    | 255.255.0.0   |
| /24    | 255.255.255.0 |

---

## Detailed Interface Inspection

```bash
show interfaces g0/0
```

Displays:

* Layer 1 and Layer 2 status
* MAC address (BIA)
* IP address
* Speed / duplex
* Errors and drops

---

## Interface Descriptions

View:

```bash
show interfaces description
```

Add description:

```bash
interface g0/0
description Connection to LAN1
```

---

## View Configuration

```bash
show running-config
```

Shorthand:

```bash
show run
```

---

## Save Configuration

```bash
copy running-config startup-config
```

```bash
write memory
```

```bash
wr
```

---

## PC Configuration (Packet Tracer)

* Assign IP manually
* Subnet mask auto-fills
* Default gateway = router interface IP

Verify:

```bash
ping <ip-address>
```

---

## Key Takeaways

* Router interfaces must have IP addresses to route traffic 
* Interfaces must be enabled with `no shutdown`
* Network and broadcast addresses are not usable
* Default gateway is required to reach other networks
* Interface status must be verified before troubleshooting

---

## Self-Check

1. What happens if an interface has no IP?
   → It cannot participate in Layer 3 routing

2. What happens if `no shutdown` is missing?
   → Interface remains down

3. What does `show ip interface brief` verify?
   → IP assignment and interface status

4. How do you find the last usable address?
   → Broadcast − 1

