# STP — Part 2 (States, Timers, PortFast, BPDU Guard)

---

## Topics Covered

* STP Port States
* STP Timers
* BPDU Fundamentals
* PortFast
* BPDU Guard
* Root Guard
* Loop Guard
* Root Primary / Secondary
* STP Load Balancing
* STP Cost and Port Priority

---

# Why This Matters

STP prevents Layer 2 loops by blocking redundant paths.

However:

```text
Network topologies change.
Links fail.
Switches are added.
Interfaces go down.
```

STP must be able to:

```text
Detect changes
Recalculate topology
Activate backup paths
Prevent new loops
```

---

# STP Port States

Ports do not instantly move from Blocking to Forwarding.

They transition through several states.

---

## Blocking State

Purpose:

```text
Prevent Layer 2 loops
```

Characteristics:

```text
Receives BPDUs
Does not forward traffic
Does not learn MAC addresses
Drops normal traffic
```

Stable state for:

```text
Non-Designated Ports
```

---

## Listening State

Purpose:

```text
Prepare to become forwarding
```

Characteristics:

```text
Sends/receives BPDUs
Does not forward traffic
Does not learn MAC addresses
```

Default duration:

```text
15 seconds
```

---

## Learning State

Purpose:

```text
Build MAC address table before forwarding traffic
```

Characteristics:

```text
Sends/receives BPDUs
Does not forward traffic
Learns MAC addresses
```

Default duration:

```text
15 seconds
```

---

## Forwarding State

Normal operating state.

Characteristics:

```text
Sends/receives BPDUs
Forwards traffic
Learns MAC addresses
```

Stable state for:

```text
Root Ports
Designated Ports
```

---

## Disabled State

Interface is:

```text
Administratively shut down
```

Example:

```bash
shutdown
```

---

# STP State Transition

When a blocked port must become forwarding:

```text
Blocking
→ Listening (15 sec)
→ Learning (15 sec)
→ Forwarding
```

Total convergence time:

```text
50 seconds
```

Calculation:

```text
20 sec Max Age
15 sec Listening
15 sec Learning
```

This slow convergence is one reason Rapid STP was developed.

---

# STP Timers

---

## Hello Timer

Purpose:

```text
How often Root Bridge sends BPDUs
```

Default:

```text
2 seconds
```

---

## Forward Delay Timer

Purpose:

```text
Controls Listening and Learning states
```

Default:

```text
15 seconds
```

Used twice:

```text
Listening = 15 sec
Learning = 15 sec
```

---

## Max Age Timer

Purpose:

```text
How long a switch waits before assuming topology changed
```

Default:

```text
20 seconds
```

Behavior:

```text
BPDU received → timer resets

No BPDU received for 20 sec
→ STP recalculates topology
```

---

# BPDUs

BPDU:

```text
Bridge Protocol Data Unit
```

Purpose:

```text
Carry STP information between switches
```

Information contained:

```text
Root Bridge ID
Root Path Cost
Bridge ID
Port ID
Timers
Topology Change Information
```

---

## Important Rule

Only switches generate and process BPDUs.

```text
PCs do not send BPDUs
Routers do not send BPDUs
```

---

# PortFast

---

## Problem

When an end host connects:

```text
Switch waits through
Listening + Learning
```

Result:

```text
30 second delay
```

Users may experience:

```text
DHCP issues
Login delays
Network connectivity delays
```

---

## Solution

PortFast allows a port to:

```text
Immediately enter Forwarding State
```

Bypasses:

```text
Listening
Learning
```

---

## Configuration

Interface:

```bash
interface g0/2
spanning-tree portfast
```

---

## Global Configuration

```bash
spanning-tree portfast default
```

Applies to:

```text
Access ports only
```

---

## Important Rule

Only enable PortFast on:

```text
PCs
Servers
Printers
End Hosts
```

Never on:

```text
Switch-to-Switch links
```

---

# BPDU Guard

---

## Purpose

Protects PortFast ports from accidental loops.

If a BPDU is received:

```text
Port immediately shuts down
```

---

## Configuration

Interface:

```bash
interface g0/1
spanning-tree bpduguard enable
```

---

## Global Configuration

```bash
spanning-tree portfast bpduguard default
```

Applies BPDU Guard to:

```text
All PortFast interfaces
```

---

## Recovery

After fixing the issue:

```bash
shutdown
no shutdown
```

If the problem still exists:

```text
Port will immediately shut down again
```

---

# Root Guard

Purpose:

```text
Prevent another switch from becoming Root Bridge
```

If a superior BPDU arrives:

```text
Port is disabled
```

Useful when:

```text
Maintaining intended STP topology
```

---

# Loop Guard

Purpose:

```text
Prevent loops caused by unidirectional link failures
```

If BPDUs suddenly stop:

```text
Port remains blocked
```

instead of forwarding.

---

# STP Modes

---

## PVST

```text
Per-VLAN Spanning Tree
```

One STP instance per VLAN.

---

## Rapid PVST

```text
Rapid Per-VLAN Spanning Tree
```

Faster convergence.

Modern Cisco switches typically use:

```text
Rapid PVST
```

by default.

---

## MST

```text
Multiple Spanning Tree
```

Allows multiple VLANs to share STP instances.

---

# Root Primary

Instead of manually calculating priorities:

```bash
spanning-tree vlan 1 root primary
```

Usually sets:

```text
24576
```

or lower than current root.

---

# Root Secondary

Backup Root Bridge:

```bash
spanning-tree vlan 1 root secondary
```

Usually sets:

```text
28672
```

---

# STP Load Balancing

---

## Question

Can different VLANs have different STP topologies?

Answer:

```text
Yes
```

Because PVST runs:

```text
One STP instance per VLAN
```

---

## Example

```text
VLAN 10 blocks Link A
VLAN 20 blocks Link B
```

Result:

```text
Both links carry traffic
```

instead of wasting bandwidth.

This is called:

```text
STP Load Balancing
```

---

# STP Cost

Purpose:

```text
Influences Root Port selection
```

Lower cost wins.

Example:

```bash
spanning-tree vlan 1 cost 10
```

---

# STP Port Priority

Purpose:

```text
Tie-breaker when costs are equal
```

Lower priority wins.

Default:

```text
128
```

Example:

```bash
spanning-tree vlan 1 port-priority 64
```

---

# Relevant Commands

---

## Verify STP

```bash
show spanning-tree
```

Shows:

```text
Root Bridge
Root Ports
Designated Ports
Blocking Ports
Costs
Roles
States
```

---

## View Specific VLAN

```bash
show spanning-tree vlan 10
```

---

## Detailed Information

```bash
show spanning-tree detail
```

Shows:

```text
Topology changes
Timers
BPDU information
Port transitions
```

---

## Summary View

```bash
show spanning-tree summary
```

Shows:

```text
STP mode
PortFast status
BPDU Guard status
Forwarding/Blocking counts
```

---

# Key Takeaways

```text
Blocking ports prevent loops
```

```text
Listening and Learning are transitional states
```

```text
PortFast skips Listening and Learning
```

```text
BPDU Guard protects PortFast ports
```

```text
Rapid PVST is the modern Cisco default
```

```text
Different VLANs can have different STP topologies
```

```text
STP Cost influences Root Port selection
```

```text
Port Priority is a tie-breaker
```

---

# Self-Check

1. What STP state learns MAC addresses but does not forward traffic?
   → Learning

2. What timer controls Listening and Learning?
   → Forward Delay

3. What does PortFast do?
   → Immediately transitions a port to Forwarding

4. What happens if BPDU Guard receives a BPDU?
   → Port shuts down

5. Why can VLAN 10 and VLAN 20 block different links?
   → PVST runs a separate STP instance per VLAN

6. What is the default STP port priority?
   → 128

7. What is the purpose of Root Guard?
   → Prevent another switch from becoming Root Bridge

8. What is the purpose of Loop Guard?
   → Prevent loops caused by unidirectional link failures
