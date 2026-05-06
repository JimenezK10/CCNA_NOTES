# VLANs — Intro 

---

## What is a LAN?

* A LAN is a group of devices in a single location.

More precise:

```text
A LAN is a single broadcast domain
```

---

## What is a Broadcast Domain?

```text
All devices that receive a broadcast frame
(destination MAC = ffff.ffff.ffff)
```

---

## Where Do Broadcast Frames Come From?

* Broadcast frames are created by hosts (example: ARP requests)
* Switches do not create broadcasts
* Switches flood broadcasts

```text
Host creates broadcast
Switch floods broadcast
```

---

## Important Note

```text
A point-to-point link is still a broadcast domain
```

---

## Problem with One Large LAN

### Performance

```text
Too many broadcasts → wasted bandwidth
```

### Security

```text
All devices can communicate directly
```

* Traffic does not pass through a router
* Security policies are not enforced

---

## Initial Solution: Subnetting

Separate departments into different subnets.

```text
Engineering → 192.168.1.0/25
Sales       → 192.168.1.128/25
```

---

## Result

```text
Different subnet → traffic must go to the router
```

---

## Packet Flow

```text
PC1 → Switch → Router → Switch → PC2
```

Frame example:

```text
Src IP: 192.168.1.1
Dst IP: 192.168.1.129

Src MAC: PC1
Dst MAC: Router (default gateway)
```

---

## Limitation of Subnetting Alone

```text
Each subnet requires a router interface
```

* Not scalable

---

## Remaining Problem at Layer 2

```text
Switch still floods broadcasts and unknown unicasts
```

Examples:

* ARP
* Unknown MAC

---

## What VLANs Do

```text
VLAN = separate broadcast domains on the same switch
```

---

## Key Effect

```text
1 switch = multiple broadcast domains
```

---

## Example

```text
Engineering VLAN → only engineering devices
Sales VLAN       → only sales devices
```

---

## Why VLANs Matter

### Performance

```text
Broadcast traffic stays inside VLAN
```

### Security

```text
Different VLANs cannot communicate directly
```

---

## Mental Model

```text
Switch without VLANs → one network
Switch with VLANs   → multiple networks
```

---

## Key Limitation

```text
Different VLANs require routing to communicate
```

---

# Inter-VLAN Routing (Concept)

```text
Inter-VLAN routing = communication between VLANs using a Layer 3 device
```

* VLANs are Layer 2 separation
* Routing is required to move traffic between them

---

# Legacy Inter-VLAN Routing

## Description

```text
One router interface per VLAN
```

Each VLAN connects to a separate physical interface on the router.

---

## Example

```text
VLAN 10 → R1 G0/0/0 → 192.168.1.1
VLAN 20 → R1 G0/0/1 → 192.168.2.1
```

* Each interface acts as the default gateway for its VLAN
* Router routes traffic between subnets

---

## Behavior

```text
PC (VLAN 10) → Switch → Router → Switch → PC (VLAN 20)
```

* Traffic must pass through the router
* Layer 3 decision occurs at the router

---

## Advantages

```text
Simple to understand
Easy to configure
```

---

## Limitations

```text
Requires one physical interface per VLAN
Not scalable
```

* More VLANs = more cables and interfaces

---

# Router-on-a-Stick (Preview)

```text
Single router interface using VLAN trunking and subinterfaces
```

* One physical link carries multiple VLANs
* More scalable than legacy method

---

# Relevant Commands

---

## Create VLAN

```bash
vlan <vlan-id>
```

Example:

```bash
vlan 10
name ENGINEERING
```

---

## View VLANs

```bash
show vlan brief
```

* Displays VLANs and assigned ports

---

## Assign Port to VLAN

```bash
interface <port>
switchport mode access
switchport access vlan <vlan-id>
```

---

## Verify Configuration

```bash
show running-config
```

---

## View MAC Address Table

```bash
show mac address-table
```

---

## Check Interface Status

```bash
show interfaces status
```

---

# Key Takeaways

```text
VLAN = Layer 2 segmentation
Routing = required for communication between VLANs
```

```text
Legacy inter-VLAN routing uses multiple physical interfaces
```

```text
Router-on-a-stick uses one interface with VLAN tagging
```

```text
Same switch does not mean same network
```

---

## Self-Check

1. What is inter-VLAN routing?
   → Communication between VLANs using a Layer 3 device

2. How does legacy inter-VLAN routing work?
   → One router interface per VLAN

3. Why is it not scalable?
   → Requires multiple physical interfaces

4. What improves scalability?
   → Router-on-a-stick

