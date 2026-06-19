# STP Election Process (Corrected)

## Step 1 — Elect the Root Bridge

Every switch starts by assuming it is the Root Bridge.

The switch with the lowest Bridge ID becomes the Root Bridge.

Bridge ID consists of:

```text
Bridge Priority
+
MAC Address
```

Election order:

```text
1. Lowest Bridge Priority
2. Lowest MAC Address
```

Default priority:

```text
32768
```

With PVST:

```text
32768 + VLAN ID
```

Example:

```text
VLAN 1  = 32769
VLAN 10 = 32778
VLAN 20 = 32788
```

---

## Step 2 — Root Bridge Ports

Once elected:

```text
All ports on the Root Bridge become Designated Ports.
```

All Designated Ports:

```text
Forwarding State
```

No Root Ports exist on the Root Bridge.

---

## Step 3 — Root Port Selection

Every non-root switch selects one Root Port.

The Root Port is:

```text
The port with the best path to the Root Bridge.
```

Selection order:

```text
1. Lowest Root Path Cost
2. Lowest Neighbor Bridge ID
3. Lowest Neighbor Port ID
```

---

## Root Path Cost

Root Path Cost is:

```text
The total STP cost required to reach the Root Bridge.
```

Common costs:

```text
10 Mbps   = 100
100 Mbps  = 19
1 Gbps    = 4
10 Gbps   = 2
```

Important:

```text
STP compares total path cost,
not whether a link is directly connected.
```

Example:

```text
SW2 F0/3 → SW3 (100 Mbps)
Cost = 19
```

vs

```text
SW2 G0/1 → SW4 → SW3
4 + 4 = 8
```

Even though:

```text
F0/3 is directly connected to the Root Bridge
```

STP chooses:

```text
G0/1 as the Root Port
```

because:

```text
8 < 19
```

This was the source of confusion in the lab.

---

## Step 4 — Designated Port Selection

Every Layer 2 segment gets:

```text
Exactly one Designated Port
```

The Designated Port is:

```text
The port advertising the best path to the Root Bridge on that segment.
```

Election order:

```text
1. Lowest Root Path Cost
2. Lowest Bridge ID
3. Lowest Port ID
```

The winner becomes:

```text
Designated Port
Forwarding State
```

---

## Step 5 — Blocking Ports

After Root Ports and Designated Ports are selected:

```text
All remaining ports become Non-Designated Ports.
```

State:

```text
Blocking
```

Blocking ports:

```text
Do not forward user traffic.
Only process STP BPDUs.
```

---

# Important Rules

## Rule 1

```text
All Root Bridge ports are Designated Ports.
```

---

## Rule 2

```text
Every non-root switch gets exactly one Root Port.
```

---

## Rule 3

```text
Every Layer 2 segment gets exactly one Designated Port.
```

---

## Rule 4

```text
Remaining ports become Blocking Ports.
```

---

## Rule 5

```text
Both sides of a link can never be blocking.
```

One side must remain Designated.

---

# Why STP Exists

Without STP:

```text
Broadcasts loop forever.
```

Because Ethernet frames have:

```text
No TTL field.
```

This causes:

### Broadcast Storms

```text
Broadcast traffic continuously loops.
```

Result:

```text
Network congestion.
```

---

### MAC Address Flapping

Switches learn MAC addresses from source frames.

If the same source MAC repeatedly appears on different interfaces:

```text
MAC table constantly changes.
```

This is called:

```text
MAC Address Flapping
```

---

# BPDUs

STP uses:

```text
BPDU
Bridge Protocol Data Unit
```

Default Hello Timer:

```text
2 seconds
```

Switches send Hello BPDUs periodically.

If a switch receives a BPDU:

```text
It knows another switch exists on that segment.
```

---

# Show Commands

## show spanning-tree

Used to view:

```text
Root Bridge
Root Port
Designated Ports
Blocking Ports
Root Cost
Port Roles
Port States
```

---

## show spanning-tree vlan <vlan-id>

Used to view:

```text
STP information for a specific VLAN.
```

Useful because:

```text
PVST runs a separate STP instance per VLAN.
```

---

## show spanning-tree detail

Used to view:

```text
Topology changes
BPDU information
Port timers
Port transitions
Root information
```

---

## show spanning-tree summary

Used to view:

```text
STP mode
PortFast status
BPDU Guard status
Loop Guard status
Number of forwarding/blocking ports
```

---

# Key Takeaway From This Lab

The biggest lesson:

```text
STP does NOT choose the Root Port based on physical proximity.
```

It chooses:

```text
The lowest total Root Path Cost.
```

A directly connected FastEthernet link can lose to a longer Gigabit path if the Gigabit path has a lower cumulative STP cost.

That explains exactly why SW2 selected G0/1 as the Root Port instead of F0/3 in your topology.
