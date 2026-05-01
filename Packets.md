# Packets

---

## Purpose

Understand what happens when traffic moves:

* Between networks
* Through routers
* Using ARP, switching, and routing

Focus is on **process**, not commands.

---

## MAC Addresses on Interfaces

* Every network interface has a MAC address
* Also called **BIA (Burned-In Address)**
* Used at Layer 2

MAC addresses are only relevant **within a local network (LAN)**.

---

## ARP (Address Resolution Protocol)

### Why ARP Exists

* Devices communicate using IP
* Ethernet requires MAC
* ARP maps:

```text id="g3qv9p"
IP → MAC
```

---

## ARP Request

* Sent when MAC is unknown
* Broadcast frame

```text id="trbnry"
Destination MAC = ffff.ffff.ffff
```

* Sent to all devices in the LAN
* Not forwarded by routers

---

## ARP Reply

* Sent by the device that owns the IP
* Unicast

After this:

```text id="q7q09l"
ARP table is updated
```

---

## Sending to a Remote Network

When a host sends traffic to another network:

### Layer 3 Decision

```text id="x2apwy"
Destination not local → send to default gateway
```

---

### Layer 2 Encapsulation

* Source MAC = host
* Destination MAC = **router (default gateway)**

---

## At the Router

When a router receives a frame:

1. Removes Layer 2 header
2. Checks destination IP
3. Searches routing table
4. Selects best match

If no match:

```text id="2c8w3n"
Packet is dropped
```

---

## Router ARP (Next Hop)

If router does not know next-hop MAC:

* Sends ARP request
* Receives ARP reply
* Stores result

Then:

* Builds new frame
* Sends packet forward

---

## Packet vs Frame

### Packet (Layer 3)

* Source IP → does not change
* Destination IP → does not change

---

### Frame (Layer 2)

* Source MAC → changes every hop
* Destination MAC → changes every hop

---

## Router Behavior

Routers:

* Remove incoming frame
* Make routing decision
* Build new frame

---

## Switch Behavior

Switches:

* Forward based on MAC
* Do not inspect IP
* Do not modify frames

If destination MAC unknown:

```text id="ntkkxa"
Flood
```

---

## Key Rules

```text id="d9t0nf"
IP = end-to-end identity
MAC = local delivery only
```

---

```text id="vcz6yj"
Routers rewrite frames
Packets stay the same
```

---

```text id="k0s97c"
ARP is local only
Never crosses a router
```

---

## Key Takeaways

* ARP resolves IP to MAC on a local network 
* Default gateway is used for remote communication
* Routers make decisions based on destination IP
* MAC addresses change at every hop
* IP addresses remain constant end-to-end

---

## Self-Check

1. What changes at every hop?
   → MAC address

2. What stays the same end-to-end?
   → IP address

3. When is ARP used?
   → When MAC is unknown

4. Does ARP cross routers?
   → No
