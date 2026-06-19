# VLANs — Part 2 (Trunks + ROAS)

---

## What This Covers

* Trunk ports
* 802.1Q tagging
* VLAN ranges
* Native VLAN
* Router-on-a-Stick (ROAS)

---

# Why Trunk Ports Exist

In small networks:

```text id="jlwm92"
One VLAN = one physical link
```

This works, but:

```text id="jlwm93"
More VLANs → more cables → not scalable
```

---

# Trunk Port

```text id="jlwm94"
Trunk port = carries multiple VLANs over a single interface
```

---

# Access vs Trunk Ports

```text id="jlwm95"
Access port = single VLAN
Trunk port = multiple VLANs
```

---

# Key Behavior

```text id="jlwm96"
Switch tags frames on trunk links
```

This allows the receiving device to identify:

```text id="jlwm97"
Which VLAN the frame belongs to
```

---

# VLAN Tagging (802.1Q)

Two trunking protocols exist:

* ISL (Cisco proprietary, deprecated)
* 802.1Q (industry standard)

---

# 802.1Q Tag Structure

A 4-byte tag is inserted into the Ethernet frame.

Placement:

```text id="jlwm98"
Between Source MAC and Type/Length field
```

---

# Tag Fields

## TPID (Tag Protocol Identifier)

```text id="jlwm99"
16 bits
Value = 0x8100
```

Indicates:

```text id="jlwm100"
Frame is 802.1Q tagged
```

---

## TCI (Tag Control Information)

Contains:

```text id="jlwm101"
PCP = priority/QoS
DEI = drop eligibility
VID = VLAN ID
```

---

# VLAN ID Range

```text id="jlwm102"
1 - 4094 usable
0 and 4095 reserved
```

---

# VLAN Types

```text id="jlwm103"
Normal VLANs   = 1 - 1005
Extended VLANs = 1006 - 4094
```

---

# Important Limitation

```text id="jlwm104"
Switches cannot route between VLANs
```

Layer 3 routing is required.

---

# Native VLAN

Default:

```text id="jlwm105"
VLAN 1
```

---

## Behavior

```text id="jlwm106"
Native VLAN traffic is NOT tagged
```

---

## Important Rule

```text id="jlwm107"
Both sides of trunk must match native VLAN
```

Mismatch can cause:

* traffic issues
* security problems

---

## Best Practice

```text id="jlwm108"
Use an unused VLAN as the native VLAN
```

Example:

```text id="jlwm109"
VLAN 1001
```

---

# Trunk Configuration

---

## Configure Trunk Port

```bash id="’wini110"
interface <port>
switchport mode trunk
```

---

## Configure Encapsulation (if required)

```bash id="’wini111"
switchport trunk encapsulation dot1q
```

(Some switches default to dot1q automatically.)

---

## Verify Trunks

```bash id="’wini112"
show interfaces trunk
```

---

# Trunk Output Breakdown

```text id="’wini113"
Mode          = trunk status
Encapsulation = dot1q
Native VLAN   = untagged VLAN
```

---

# Allowed VLANs on Trunk

Default:

```text id="’wini114"
All VLANs allowed
```

---

## Restrict Allowed VLANs

```bash id="’wini115"
switchport trunk allowed vlan 10,20
```

---

## Additional Options

```text id="’wini116"
add
remove
all
none
except
```

---

# Verify Allowed VLANs

```bash id="’wini117"
show interfaces trunk
```

---

# Router-on-a-Stick (ROAS)

---

# Concept

```text id="’wini118"
One router interface handles multiple VLANs
```

---

# How It Works

* Switch port = trunk
* Router uses subinterfaces

---

# Subinterfaces

Example:

```text id="’wini119"
g0/0.10
g0/0.20
```

---

# Configuration Steps

---

## Step 1 — Enable Physical Interface

```bash id="’wini120"
interface g0/0
no shutdown
```

---

## Step 2 — Create Subinterface

```bash id="’wini121"
interface g0/0.10
```

---

## Step 3 — Assign VLAN Tag

```bash id="’wini122"
encapsulation dot1q 10
```

---

## Step 4 — Assign IP Address

```bash id="’wini123"
ip address 192.168.1.1 255.255.255.0
```

This becomes:

```text id="’wini124"
Default gateway for VLAN 10
```

---

# Repeat for Additional VLANs

```bash id="’wini125"
interface g0/0.20
encapsulation dot1q 20
ip address 192.168.2.1 255.255.255.0
```

---

# Important Note

```text id="’wini126"
Subinterface number should match VLAN ID
```

(Not required, but best practice.)

---

# Verification Commands

```bash id="’wini127"
show ip interface brief
```

```bash id="’wini128"
show ip route
```

---

# ROAS Traffic Flow

```text id="’wini129"
PC → switch access port
→ trunk link
→ router subinterface
→ routing decision
→ router subinterface
→ trunk link
→ destination VLAN
```

---

# ROAS vs Legacy Inter-VLAN Routing

## Legacy

```text id="’wini130"
One physical router interface per VLAN
```

---

## ROAS

```text id="’wini131"
One physical router interface for many VLANs
```

---

# Advantages of ROAS

```text id="’wini132"
Fewer interfaces
Fewer cables
More scalable
```

---

# Limitations of ROAS

```text id="’wini133"
All inter-VLAN traffic crosses one physical link
```

Can become a bottleneck in larger networks.

---

# Troubleshooting Learned in Lab

---

# Issue 1 — VLAN Missing on Switch

Problem:

```text id="’wini134"
VLAN allowed on trunk but not created on switch
```

Result:

```text id="’wini135"
Tagged traffic dropped
ARP failed
```

---

## Important Rule

```text id="’wini136"
Allowed on trunk ≠ VLAN exists
```

Both must be true:

```text id="’wini137"
1. VLAN created
2. VLAN allowed on trunk
```

---

# Issue 2 — Wrong Endpoint Port Type

Problem:

```text id="’wini138"
Endpoint configured on trunk port
```

Result:

```text id="’wini139"
Traffic entered wrong/native VLAN
```

---

## Correct Fix

```bash id="’wini140"
switchport mode access
switchport access vlan <id>
```

---

# Issue 3 — Native VLAN Mismatch

Problem:

```text id="’wini141"
Native VLAN mismatch across trunk
```

Fix:

```bash id="’wini142"
switchport trunk native vlan 1001
```

Both sides must match.

---

# Key Takeaways

```text id="’wini143"
Access port = single VLAN
Trunk port = multiple VLANs
```

```text id="’wini144"
802.1Q tagging allows multiple VLANs over one link
```

```text id="’wini145"
ROAS enables inter-VLAN routing using one router interface
```

```text id="’wini146"
Native VLAN traffic is untagged
```

---

# Self-Check

1. What does a trunk port do?
   → Carries multiple VLANs

2. What protocol is used for VLAN tagging?
   → 802.1Q

3. What is the native VLAN?
   → VLAN whose traffic is untagged

4. Why use ROAS?
   → Inter-VLAN routing with fewer physical interfaces

5. What happens if VLANs are allowed on a trunk but not created locally?
   → Traffic is dropped
