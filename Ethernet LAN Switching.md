# Ethernet LAN Switching — Part 1

## Layer Overview

Ethernet switching operates at the **Data Link Layer (Layer 2)** of the OSI model.

* Provides **node-to-node communication**
* Uses **MAC addressing (physical addressing)**
* Detects transmission errors (does not correct them)

### Devices

* **Switches** operate at Layer 2 and forward traffic within the same LAN
* **Routers** operate at Layer 3 and connect separate networks

---

## Mental Model

```
Same LAN → Switch (MAC-based forwarding)
Different LAN → Router (IP-based routing)
```

---

## Protocol Data Units (PDUs)

* A **PDU (Protocol Data Unit)** at Layer 2 is called a **Frame**

### Encapsulation

```
[ Ethernet Header ] [ Layer 3 Packet ] [ Ethernet Trailer ]
```

Ethernet is unique because it includes both a **header and a trailer**.

---

## Ethernet Frame Structure

```
[Preamble][SFD][Destination MAC][Source MAC][Type/Length][DATA][FCS]
```

---

## Field Breakdown

| Field           | Purpose               |
| --------------- | --------------------- |
| Preamble        | Synchronization       |
| SFD             | Start of frame        |
| Destination MAC | Receiving device      |
| Source MAC      | Sending device        |
| Type/Length     | Payload type or size  |
| FCS             | Error detection (CRC) |

---

## Preamble & SFD

### Preamble

* 7 bytes
* Pattern: `10101010`
* Used for clock synchronization

### SFD (Start Frame Delimiter)

* 1 byte
* Pattern: `10101011`
* Indicates the start of the actual frame

---

## MAC Addresses

* 48 bits (6 bytes)
* Represented in hexadecimal:

```
00:1A:2B:3C:4D:5E
```

### Structure

* First 3 bytes → **OUI (vendor identifier)**
* Last 3 bytes → **unique device identifier**

---

## Type / Length Field

* 2 bytes

### Interpretation

| Value  | Meaning                |
| ------ | ---------------------- |
| ≤ 1500 | Payload length (bytes) |
| ≥ 1536 | Layer 3 protocol type  |

### Examples

* IPv4 → `0x0800`
* IPv6 → `0x86DD`

---

## Frame Check Sequence (FCS)

* 4 bytes
* Uses **CRC (Cyclic Redundancy Check)**

### Behavior

* If an error is detected → frame is **discarded**
* Ethernet does not retransmit

---

## MAC Address Basics

* Assigned at manufacture
* Also called **Burned-In Address (BIA)**
* Globally unique

---

## Hexadecimal Refresher

```
0–9, A–F
```

* Each hex digit = 4 bits

---

# Core Switching Logic

## MAC Address Table

Switches maintain a table:

```
MAC Address → Interface (Port)
```

---

## MAC Address Learning

When a frame enters a switch:

1. The switch reads the **source MAC address**
2. It stores or updates the entry:

```
Source MAC → Incoming Port
```

### Key Rule

Switches **always learn from the source MAC address**, even if the MAC already exists in the table. Existing entries are refreshed.

---

## Unicast Forwarding Behavior

### Step-by-Step Logic

```
Frame arrives →
    Learn source MAC →
        Check destination MAC →
```

---

### Known Unicast

* Destination MAC exists in table

```
→ Forward out the specific interface only
```

---

### Unknown Unicast

* Destination MAC not in table

```
→ Flood out all interfaces except the incoming interface
```

---

## Critical Rules

* Switches **only learn from source MAC addresses**
* Flooding **never includes the incoming port**
* Known unicast = **single-port forwarding**
* Unknown unicast = **flooding behavior**

---

## MAC Address Aging

* Default aging time: ~5 minutes of inactivity
* Removes stale entries from the table
* If traffic is seen again, the entry is refreshed and the timer resets

---

# Verification and Management Commands

These commands are used to observe and manage MAC address behavior on a switch.

## View MAC Address Table

```
show mac address-table
```

Displays:

* Learned MAC addresses
* Associated interfaces
* Entry types (dynamic/static)

---

## View Specific MAC Address Entry

```
show mac address-table address <mac-address>
```

---

## Clear MAC Address Table

```
clear mac address-table dynamic
```

Removes all dynamically learned MAC addresses.

---

## View Interface Status

```
show interfaces status
```

Useful for confirming:

* Active ports
* Link state
* Interface roles

---

## Key Takeaways

* Switching decisions are based entirely on **MAC addresses**
* The switch learns from the **source MAC**, not the destination
* Unknown destinations result in **flooding**
* Known destinations result in **targeted forwarding**
* The MAC table is **dynamic and constantly updating**

---

## Self-Check

You should be able to answer these immediately:

1. How does a switch learn MAC addresses?
   → From the source MAC of incoming frames

2. What happens if the destination MAC is unknown?
   → The frame is flooded out all ports except the incoming port

3. Does a switch ever learn from the destination MAC?
   → No

