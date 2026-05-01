# Switch Interfaces

---

## What to Cover

Key areas for switch interfaces:

* Speed (bits per second)
* Duplex (transmit/receive behavior)
* Auto-negotiation
* Interface status
* Interface errors and counters

---

## Switch Interfaces vs Router Interfaces

* Switches have many interfaces to connect end devices
* Routers typically have fewer interfaces

Switch ports:

* Operate at **Layer 2**
* Do **not require IP addresses** to forward traffic
* Forward frames based on **MAC addresses**

IP addressing on switches is used later for management (SVIs), not forwarding.

---

## Viewing Interfaces (Basic)

```bash
show ip interface brief
```

On a switch:

* Most interfaces show:

```text
IP address = unassigned
```

* Connected ports:

```text
up/up
```

* Unused ports:

```text
down/down
```

### Important Distinction

* `down/down` → not connected
* `administratively down/down` → manually shut

Default behavior:

* Routers → interfaces shutdown by default
* Switches → interfaces enabled by default

---

## Interface Summary (Preferred Command)

```bash
show interfaces status
```

(Works on switches only)

### Fields

* Port → interface ID
* Name → description
* Status → connected / notconnect / disabled
* Vlan → VLAN assignment
* Duplex → auto / full / half
* Speed → negotiated speed
* Type → physical media

### Status Meaning

* `connected` → link is up
* `notconnect` → no device connected
* `disabled` → manually shut

---

## Duplex

### Half-Duplex

* Cannot send and receive at the same time
* Collisions can occur
* Used with hubs

---

### Full-Duplex

* Can send and receive simultaneously
* No collisions
* Standard in modern networks

---

## CSMA/CD (Legacy Concept)

Used only in half-duplex:

1. Listen before sending
2. Transmit
3. Detect collision
4. Back off and retry

Not used in full-duplex networks.

---

## Speed and Duplex Auto-Negotiation

Default:

```text
speed auto
duplex auto
```

### Behavior

* Devices advertise capabilities
* Highest common speed is selected
* Duplex defaults to full if supported

### Where It Happens

* Configured on **both ends of the link**
* Each interface negotiates with its neighbor

---

## Mismatch Scenario

If one side is manual and the other is auto:

* Speed may still match
* Duplex may not

Example:

* Device → 100 Mbps full
* Switch → auto

Result:

* Switch detects speed
* Defaults to **half-duplex**
* Device stays **full-duplex**

Outcome:

* Collisions
* CRC errors

---

## Packet Tracer Behavior

In Cisco Packet Tracer:

* Mismatch can cause:

```text
link down/down
```

In real hardware:

* Link often stays up
* Errors appear instead

---

## Best Practice

* Use:

```text
auto / auto
```

* If manually configured:

```text
Both sides must match exactly
```

---

## Manual Configuration

```bash
configure terminal
interface f0/1
speed 100
duplex full
```

---

## Verification

```bash
show interfaces status
```

---

## Interface Descriptions

```bash
interface f0/1
description ## to R1 ##
```

Verify:

```bash
show interfaces status
```

---

## Trunk Mode (Intro Only)

```bash
switchport mode trunk
```

* Allows multiple VLANs on a single link
* Used between switches or switch-router links

---

## Securing Unused Ports

Unused ports are enabled by default.

Best practice:

```bash
interface range f0/5 - 24
description ## not in use ##
shutdown
```

Non-consecutive:

```bash
interface range f0/5 - 6, f0/9 - 12
```

---

## Interface Error Counters

```bash
show interfaces f0/1
```

### Key Counters

* Runts → frames < 64 bytes
* Giants → frames > 1518 bytes
* CRC → failed FCS check
* Input errors → receive issues
* Output errors → send issues
* Collisions → duplex mismatch indicator

---

## Key Takeaways

* Switch interfaces operate at Layer 2 and do not require IPs 
* Auto-negotiation should be used unless troubleshooting
* Duplex mismatches cause performance issues or link failure
* Interface status must be verified before troubleshooting
* Error counters provide insight into physical/data link problems

---

## Self-Check

1. What happens if duplex does not match?
   → Collisions and errors occur

2. What does `show interfaces status` display?
   → Interface state, speed, duplex, VLAN

3. Why are unused ports shut down?
   → Security

4. Does a switch need an IP to forward traffic?
   → No
