Added as a **PortFast Addendum** section in your STP Part 2 notes.

---

# PortFast on Trunk Links

---

## Common Rule

Normally:

```text id="1mbdr6"
PortFast is enabled on access ports connected to end hosts.
```

Examples:

* PCs
* Printers
* Workstations
* End-user devices

---

## Important Exception

PortFast can also be used on certain trunk links.

This is valid when the device connected to the trunk:

```text id="drd2zd"
Cannot create a Layer 2 loop
```

---

# Example 1 — Virtualization Servers

Modern virtualization servers often host:

```text id="4zjlwm"
Multiple virtual machines
```

Each VM may belong to a different VLAN.

As a result:

```text id="jlwm50"
Server-to-switch links are often configured as trunks
```

instead of access ports.

Since the server is not participating in STP:

```text id="jlwm51"
PortFast can be safely enabled
```

---

# Example 2 — Router-on-a-Stick (ROAS)

ROAS uses:

```text id="jlwm52"
Switch ↔ Router trunk connection
```

The trunk carries multiple VLANs.

However:

```text id="jlwm53"
Routers do not create Layer 2 loops
```

Routers:

```text id="jlwm54"
Do not flood Layer 2 frames
Do not participate in STP
```

Therefore:

```text id="jlwm55"
PortFast can safely be enabled
```

on the switch port connected to the router.

---

# Configuring PortFast on a Trunk

PortFast trunk must be configured per interface.

```bash
interface g0/1
spanning-tree portfast trunk
```

---

# Verification

```bash
show running-config interface g0/1
```

---

# PortFast Edge

Modern Cisco switches use:

```text id="jlwm56"
PortFast Edge
```

This is the current implementation used in Cisco IOS.

---

## Automatic Conversion

If you enter:

```bash
spanning-tree portfast
```

IOS may display:

```bash
spanning-tree portfast edge
```

---

If you enter:

```bash
spanning-tree portfast trunk
```

IOS may display:

```bash
spanning-tree portfast edge trunk
```

---

If you enter:

```bash
spanning-tree portfast default
```

IOS may display:

```bash
spanning-tree portfast edge default
```

---

## Key Point

```text id="jlwm57"
Edge and PortFast are effectively the same thing for CCNA purposes.
```

The switch simply displays the newer terminology.

---

# PortFast Network

Another mode exists:

```text id="jlwm58"
PortFast Network
```

This is used with:

```text id="jlwm59"
Bridge Assurance
```

However:

```text id="jlwm60"
PortFast Network is not a CCNA topic.
```

Focus on:

```text id="jlwm61"
PortFast Edge
```

---

# Disable PortFast

```bash
interface g0/1
spanning-tree portfast disable
```

---

# Useful Command

Display configuration for a single interface:

```bash
show running-config interface g0/1
```

Useful for verifying:

* PortFast
* BPDU Guard
* Trunking
* Access VLAN assignments

---

# Key Takeaways

```text id="jlwm62"
PortFast is normally used on access ports.
```

```text id="jlwm63"
PortFast can also be used on trunk links connected to devices that cannot create Layer 2 loops.
```

```text id="jlwm64"
Common examples are virtualization servers and ROAS routers.
```

```text id="jlwm65"
Modern Cisco switches automatically use the Edge keyword.
```

```text id="jlwm66"
PortFast Network and Bridge Assurance are outside normal CCNA scope.
```

This is one of those small STP details that shows up on exams because Cisco loves exceptions to the "PortFast only on access ports" rule. The ROAS example is the one worth remembering because you've actually built it in your labs.
