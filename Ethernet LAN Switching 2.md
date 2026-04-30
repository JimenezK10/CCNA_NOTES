# Ethernet LAN Switching — Part 2

---

## Ethernet Frame Sizing

Switches operate at Layer 2 and forward frames within a LAN. Understanding frame size is important for both exams and troubleshooting.

* **Preamble + SFD** are used for synchronization on the wire
  → Not counted as part of the Ethernet header in most discussions

### Key Sizes

* **Ethernet Header (Destination MAC + Source MAC + Type/Length)** = **14 bytes**

* **FCS (Frame Check Sequence)** = **4 bytes**

* **Total Layer 2 Overhead** = **18 bytes**

* **Minimum Ethernet Frame Size (Dest MAC → FCS)** = **64 bytes**

* **Minimum Payload Size** = **46 bytes**

### Padding

If the payload is less than 46 bytes, padding is added:

```text
Example:
34 bytes data + 12 bytes padding = 46 bytes minimum payload
```

### Key Takeaway

* Header = 14 bytes
* FCS = 4 bytes
* Total overhead = 18 bytes

---

## Why ARP Exists

When sending data on a LAN:

* IP packet contains:

  * Source IP
  * Destination IP

* Ethernet frame requires:

  * Source MAC
  * Destination MAC

Problem:

* Devices usually know the destination IP, not the MAC

Solution:

* **ARP (Address Resolution Protocol)** resolves:

```text
IP Address → MAC Address
```

---

## ARP Basics

ARP is used to find the MAC address of a device on the same subnet.

### ARP Process

#### 1. ARP Request

* Sent as a **broadcast**
* Destination MAC:

```text
ffff.ffff.ffff
```

#### 2. ARP Reply

* Sent as a **unicast**
* Returned directly to the requester

---

## ARP Table (Cache)

After resolution, the device stores:

```text
IP Address ↔ MAC Address
```

### View ARP Table (Host)

```bash
arp -a
```

### Entry Types

* **dynamic** → learned automatically
* **static** → manually configured or system-defined

---

## ARP Happens Before Ping

When sending a ping:

1. Device checks ARP table
2. If MAC is unknown → sends ARP request
3. Receives ARP reply
4. Then sends ICMP traffic

### Key Behavior

* First ping may fail due to ARP resolution delay

---

## Ping (ICMP)

Used to test connectivity between devices.

```bash
ping <ip-address>
```

* Uses:

  * ICMP Echo Request
  * ICMP Echo Reply

---

## ARP on Cisco Devices

### View ARP Table

```bash
show arp
```

* Requires privileged EXEC mode

---

## MAC Address Table (Switch)

Switches maintain a MAC address table:

```text
MAC Address → Interface (Port) + VLAN
```

### Learning Behavior

* Switch reads **source MAC**
* Stores:

```text
Source MAC → Incoming Port
```

* Entry is refreshed when seen again

---

## View MAC Address Table

```bash
show mac address-table
```

---

## MAC Address Aging

* Default: ~300 seconds (5 minutes)
* If no traffic is seen → entry is removed
* If traffic is seen → timer resets

---

## Clearing MAC Address Table

### Clear Dynamic Entries

```bash
clear mac address-table dynamic
```

### Important Note

* Clearing a specific MAC address is **platform-dependent**
* May work on real devices or GNS3
* May **not work in Cisco Packet Tracer**

---

## Wireshark (Traffic Visibility)

Using Wireshark:

* ARP traffic shows as:

  * **ARP Request**
  * **ARP Reply**

* Ping traffic shows as:

  * **ICMP Echo Request**
  * **ICMP Echo Reply**

### Key Observation

* ARP occurs **before** ICMP when MAC is unknown

---

## Final Mental Model

```text
Send traffic →
    Check ARP table →
        MAC known → send frame
        MAC unknown → ARP request →
            learn MAC →
                send frame
```

---

## Key Takeaways

* Ethernet requires MAC addresses for delivery
* ARP resolves IP to MAC on local networks
* ARP request = broadcast, ARP reply = unicast
* Switches learn from **source MAC only**
* MAC and ARP tables are dynamic and age out over time

---

## Self-Check

1. What does ARP do?
   → Maps IP address to MAC address

2. What type of traffic is an ARP request?
   → Broadcast

3. What happens before a ping if MAC is unknown?
   → ARP request is sent

4. How does a switch learn MAC addresses?
   → From the source MAC of incoming frames

