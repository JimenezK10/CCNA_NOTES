# VLANs — Part 3 (Native VLANs + Layer 3 Switching)



---

## Topics Covered

* Native VLAN on router
* Wireshark concepts
* Layer 3 switches / multilayer switching
* Routed ports
* SVIs on L3 switches
* Default routes on L3 switches
* Troubleshooting VLAN/SVI issues

---

# Native VLAN

---

## Best Practice

```text id="b4i1x7"
Set native VLAN to an unused VLAN
```

Reason:

```text id="0tkkpx"
Native VLANs can create security issues
```

---

## Native VLAN Behavior

```text id="r8cc5o"
Native VLAN traffic is untagged
```

This slightly reduces overhead because:

```text id="u2gw8n"
802.1Q tag is not added
```

---

# Native VLAN on Router

Two methods exist.

---

## Method 1 — Native VLAN on Subinterface

```bash id="aw5qja"
interface g0/0.10
encapsulation dot1q 10 native
ip address 192.168.10.1 255.255.255.0
```

Meaning:

```text id="mx8zj7"
Untagged traffic belongs to VLAN 10
```

---

## Method 2 — Native VLAN on Physical Interface

Instead of creating a subinterface for the native VLAN:

```bash id="8t0v5m"
interface g0/0
ip address 192.168.10.1 255.255.255.0
```

Then additional VLANs use subinterfaces:

```bash id="rfpmhu"
interface g0/0.20
encapsulation dot1q 20

interface g0/0.30
encapsulation dot1q 30
```

---

# Layer 3 Switches (Multilayer Switches)

---

## What Is a Layer 3 Switch?

```text id="n6ydxr"
Layer 3 switch = switch + router functionality
```

Capabilities:

* Layer 2 switching
* Layer 3 routing
* SVIs
* Routed ports

---

## Key Difference from Layer 2 Switch

```text id="2u5q8r"
Layer 2 switch → switching only
Layer 3 switch → switching + routing
```

---

## Why Use Layer 3 Switching?

ROAS works well for small networks.

However:

```text id="u7k4ie"
All inter-VLAN traffic must cross the router link
```

This can cause:

```text id="azbb7f"
Congestion and bottlenecks
```

---

## Preferred Enterprise Method

```text id="iwjlwm"
Use Layer 3 switches for inter-VLAN routing
```

Reason:

```text id="t1jlwm"
Hardware switching ASICs route traffic faster
```

---

# Switch Virtual Interfaces (SVIs)

---

## Definition

```text id="jlwmn1"
SVI = virtual Layer 3 interface for a VLAN
```

Example:

```bash id="jlwmn2"
interface vlan 10
ip address 192.168.10.1 255.255.255.240
no shutdown
```

---

## Important Rule

```text id="jlwmn3"
Hosts use the SVI as their default gateway
```

NOT the router.

---

## Traffic Flow

```text id="jlwmn4"
PC → Layer 3 switch → routing → destination VLAN
```

The L3 switch performs:

```text id="jlwmn5"
Inter-VLAN routing internally
```

---

# Routed Ports on Layer 3 Switches

---

## Purpose

Used for:

```text id="jlwmn6"
Point-to-point Layer 3 connections
```

Usually toward:

* Routers
* Core devices
* WAN edge

---

## Convert Interface to Routed Port

```bash id="jlwmn7"
interface g0/1
no switchport
```

Meaning:

```text id="jlwmn8"
Interface becomes Layer 3 routed interface
```

NOT a switchport.

---

## Assign IP Address

```bash id="jlwmn9"
ip address 192.168.10.82 255.255.255.252
```

---

## Verification

```bash id="jlwmo0"
show interfaces status
```

You should see:

```text id="jlwmo1"
routed
```

instead of a VLAN assignment.

---

# Enable Routing on Layer 3 Switch

Critical command:

```bash id="jlwmo2"
ip routing
```

Without this:

```text id="jlwmo3"
SVIs exist but routing will not occur
```

---

# Default Route on Layer 3 Switch

Example:

```bash id="jlwmo4"
ip route 0.0.0.0 0.0.0.0 192.168.10.81
```

Meaning:

```text id="jlwmo5"
Send unknown traffic toward router
```

---

# Example Topology

```text id="jlwmo6"
PCs → Access VLANs → Layer 3 switch
                           ↓
                    Routed uplink (/30)
                           ↓
                         Router
                           ↓
                        WAN/Internet
```

---

# SVI Requirements (Critical)

For an SVI to become:

```text id="jlwmo7"
up/up
```

ALL conditions must be true:

---

## 1. VLAN Must Exist

```bash id="jlwmo8"
vlan 10
```

---

## 2. Active Port Required

Must have:

* access port in VLAN
  OR
* trunk carrying VLAN

AND interface must be:

```text id="jlwmo9"
up/up
```

---

## 3. VLAN Must Not Be Shutdown

---

# Troubleshooting Learned in Lab

---

# Issue 1 — VLAN Exists on Trunk but Not Switch

Problem:

```text id="ҩсjlwm"
Trunk allowed VLAN 30
But VLAN 30 did not exist on switch
```

Result:

```text id="h4jlwm"
Tagged traffic dropped
ARP failed
```

---

## Important Rule

```text id="jwjlwm"
Allowed on trunk ≠ VLAN exists
```

Both must be true:

```text id="vljlwm"
1. VLAN created
2. VLAN allowed on trunk
```

---

# Issue 2 — Wrong VLAN/Subnet Mapping

Problem:

```text id="7xjlwm"
VLAN 30 assigned wrong subnet/gateway
```

Example mismatch:

```text id="kwjlwm"
VLAN 30 using .64 network
But router subinterface used .32 network
```

Result:

```text id="nrjlwm"
ARP and routing failure
```

---

## Important Rule

```text id="o7jlwm"
VLAN ID, subnet, gateway, and subinterface must align
```

---

# Issue 3 — Duplicate IP Address

Problem:

```text id="2kjlwm"
Two devices assigned same IP
```

Result:

```text id="jlwm13"
ARP instability and intermittent communication
```

---

# Issue 4 — Trunk Used for Endpoint Device

Problem:

```text id="jlwm14"
Directly connected endpoint configured as trunk
```

Result:

```text id="jlwm15"
Traffic treated as native VLAN / wrong VLAN
```

---

## Correct Fix

Endpoint ports should usually be:

```bash id="jlwm16"
switchport mode access
switchport access vlan <id>
```

---

# Issue 5 — Native VLAN Mismatch

Problem:

```text id="jlwm17"
Native VLAN mismatch between switches/L3 switch
```

Fix:

```bash id="jlwm18"
switchport trunk native vlan 1001
```

Both sides must match.

---

# Modern Enterprise Design Insight

Originally:

```text id="jlwm19"
Router owned VLAN gateways (ROAS)
```

Later:

```text id="jlwm20"
Layer 3 switch owned VLAN gateways (SVIs)
```

Result:

```text id="jlwm21"
Router required static routes to reach VLAN networks
```

Because:

```text id="jlwm22"
Networks were no longer directly connected to router
```

---

# Key Architecture Shift

---

## ROAS Design

```text id="jlwm23"
Router owns VLANs
```

---

## Layer 3 Switching Design

```text id="jlwm24"
Layer 3 switch owns VLANs
Router acts as upstream/WAN path
```

---

# Relevant Commands

---

## Enable Routing on L3 Switch

```bash id="jlwm25"
ip routing
```

---

## Create SVI

```bash id="jlwm26"
interface vlan <id>
ip address <ip> <mask>
no shutdown
```

---

## Convert to Routed Port

```bash id="jlwm27"
interface g0/1
no switchport
ip address <ip> <mask>
```

---

## Create Default Route

```bash id="jlwm28"
ip route 0.0.0.0 0.0.0.0 <next-hop>
```

---

## Verify Interfaces

```bash id="jlwm29"
show ip interface brief
```

---

## Verify Routes

```bash id="jlwm30"
show ip route
```

---

## Verify VLANs

```bash id="jlwm31"
show vlan brief
```

---

## Verify Trunks

```bash id="jlwm32"
show interfaces trunk
```

---

# Key Takeaways

```text id="jlwm33"
SVIs provide gateway functionality for VLANs
```

```text id="jlwm34"
Layer 3 switches perform inter-VLAN routing internally
```

```text id="jlwm35"
Routed ports are Layer 3 interfaces, not switchports
```

```text id="jlwm36"
VLANs must exist on switches carrying VLAN traffic
```

```text id="jlwm37"
VLAN ID, subnet, gateway, and tagging must align
```

---

## Self-Check

1. What enables routing on an L3 switch?
   → `ip routing`

2. What is an SVI?
   → Virtual Layer 3 interface for a VLAN

3. Why use L3 switches over ROAS in larger networks?
   → Better scalability and performance

4. What does `no switchport` do?
   → Converts interface into routed Layer 3 port

5. Why did the router need static routes later?
   → VLAN gateways moved to the Layer 3 switch
