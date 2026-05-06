# Packets

---

## Purpose

Understand what happens when traffic moves:

* From one network to another
* Across routers
* Using ARP, switching, and routing decisions

This is about **process**, not commands.

---

## MAC Addresses (Layer 2)

* Every interface has a MAC address (BIA)
* Used only for **local delivery (LAN)**

```text
MAC = local delivery only
```

---

## ARP (Address Resolution Protocol)

### Why ARP Exists

Devices know IP addresses, but Ethernet requires MAC addresses.

```text
IP → MAC
```

---

## ARP Request

* Sent when MAC is unknown
* Broadcast frame

```text
Destination MAC = ffff.ffff.ffff
```

* Only within the **local LAN**
* Never crosses a router

---

## ARP Reply

* Unicast response
* Sent back to requester

After this:

```text
ARP table updated
```

---

## Sending to a Remote Network (Host)

### Step 1 — Layer 3 Decision

```text
Destination not in subnet → send to default gateway
```

---

### Step 2 — Layer 2 Frame

```text
Source MAC = host
Destination MAC = router (gateway)
```

---

## At the Router

When the router receives the frame:

1. Removes Layer 2 header
2. Checks destination IP
3. Searches routing table
4. Selects best match

---

## Routing Decision (Important)

```text
Match destination →
    Choose longest prefix →
        Identify next-hop
```

---

## How Router Determines Interface (Critical)

If route is:

```bash
ip route 192.168.1.0 255.255.255.0 10.0.12.1
```

Router does:

```text
1. Match destination network
2. See next-hop IP (10.0.12.1)
3. Look up next-hop in routing table
4. Determine outgoing interface
```

---

## Key Concept

```text
Routing decides WHERE
ARP decides HOW
```

---

## Router ARP (Next Hop)

If router does not know next-hop MAC:

```text
Send ARP request on the selected interface ONLY
```

Not:

* All interfaces ❌
* Entire network ❌

---

## Frame Rebuild

Router builds a new frame:

```text
Source MAC = router outgoing interface
Destination MAC = next-hop device
```

---

## Packet vs Frame

### Packet (Layer 3)

```text
Source IP → stays the same
Destination IP → stays the same
```

---

### Frame (Layer 2)

```text
Source MAC → changes every hop
Destination MAC → changes every hop
```

---

## Switch Behavior

Switches:

* Forward based on MAC
* Do not inspect IP
* Do not modify frames

If destination unknown:

```text
Flood
```

---

## Full Packet Flow (End-to-End)

```text
PC →
    ARP for gateway →
        send frame to router →

Router →
    remove frame →
    check routing table →
    determine next-hop →
    ARP if needed →
    build new frame →
    forward →

Repeat per router →

Final router →
    ARP for destination host →
    send frame →

Destination receives packet
```

---

## Key Rules

```text
IP = end-to-end identity
MAC = hop-to-hop delivery
```

---

```text
Routers rewrite frames
Packets stay the same
```

---

```text
ARP is local only
Occurs per hop
```

---

## Key Takeaways

* ARP resolves IP to MAC on each local segment
* Routers make decisions using destination IP
* Router determines interface via routing table (recursive lookup)
* ARP is only sent on the selected interface
* MAC changes every hop, IP does not

---

## Self-Check

1. What determines the outgoing interface?
   → Routing table (next-hop lookup)

2. Where is ARP sent?
   → Only on the selected interface

3. What changes every hop?
   → MAC address

4. What stays the same end-to-end?
   → IP address
