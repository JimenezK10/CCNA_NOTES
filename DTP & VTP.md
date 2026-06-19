# DTP + VTP

---

# Topics Covered

* DTP (Dynamic Trunking Protocol)
* VTP (VLAN Trunking Protocol)
* Dynamic trunk negotiation
* VTP modes
* VTP revision numbers
* VTP domains
* Why DTP and VTP are rarely used in modern networks
* Enterprise best practices

---

# DTP (Dynamic Trunking Protocol)

---

## What Is DTP?

```text
DTP = Cisco protocol used to automatically negotiate trunk links
```

Instead of manually configuring a port as:

```text
Access
or
Trunk
```

the switches can negotiate it automatically.

---

## Simple Analogy

Think of DTP like two people deciding what type of road to build between two towns.

```text
Access Port = local neighborhood road
Trunk Port = multi-lane highway carrying many VLANs
```

DTP allows the switches to negotiate:

```text
"What type of connection should we build?"
```

---

# Why DTP Exists

Without DTP:

```text
Every trunk must be configured manually
```

DTP was designed to simplify deployment.

However:

```text
Automatic behavior creates security and troubleshooting issues
```

---

# DTP Modes

---

## Access Mode

```bash
switchport mode access
```

Meaning:

```text
Force port to remain an access port
```

Carries:

```text
One VLAN only
```

---

## Trunk Mode

```bash
switchport mode trunk
```

Meaning:

```text
Force port to become a trunk
```

Carries:

```text
Multiple VLANs
```

---

## Dynamic Desirable

```bash
switchport mode dynamic desirable
```

Meaning:

```text
Actively attempts to form a trunk
```

---

### Analogy

```text
Aggressive negotiator
```

The switch says:

```text
"I want this link to become a trunk."
```

---

### Forms Trunk With

```text
trunk
dynamic desirable
dynamic auto
```

---

## Dynamic Auto

```bash
switchport mode dynamic auto
```

Meaning:

```text
Passive mode
Waits for another device to request trunking
```

---

### Analogy

```text
Shy negotiator
```

The switch says:

```text
"If you want a trunk, I'll agree."
```

---

### Forms Trunk With

```text
trunk
dynamic desirable
```

NOT:

```text
dynamic auto
```

---

# DTP Outcome Matrix

| Side A    | Side B    | Result |
| --------- | --------- | ------ |
| trunk     | trunk     | trunk  |
| trunk     | desirable | trunk  |
| trunk     | auto      | trunk  |
| desirable | desirable | trunk  |
| desirable | auto      | trunk  |
| auto      | auto      | access |
| access    | access    | access |

---

# Why Auto + Auto Fails

Both switches are waiting.

```text
Neither side initiates trunk formation
```

Result:

```text
No trunk created
```

---

# Why Trunk + Access Causes Problems

One side sends:

```text
Tagged frames
```

The other side expects:

```text
Untagged frames
```

Result:

```text
Communication issues
```

---

# Disable DTP

Best practice:

```bash
switchport nonegotiate
```

Reason:

```text
Predictable interfaces
Less risk
Easier troubleshooting
```

---

# Enterprise Best Practice

Modern networks typically use:

```text
switchport mode access

or

switchport mode trunk
```

Manually.

DTP is usually disabled.

---

# VTP (VLAN Trunking Protocol)

---

# What Is VTP?

```text
VTP = VLAN database synchronization protocol
```

It distributes VLAN information between switches.

---

## Simple Analogy

Imagine 20 switches in a building.

Without VTP:

```text
Create VLAN 10
Create VLAN 20
Create VLAN 30
```

on all 20 switches.

---

With VTP:

```text
Create VLAN once
```

and it propagates automatically.

---

# VTP Components

For VTP to function:

```text
Domain names must match
```

Switches exchange:

```text
VLAN database
Revision number
VTP version
```

---

# VTP Domain

Think of a VTP domain as:

```text
A company name
```

Only switches in the same company share updates.

---

## Configure VTP Domain

```bash
vtp domain COMPANY
```

Verify:

```bash
show vtp status
```

---

# VTP Modes

---

## Server Mode

Default mode.

Capabilities:

```text
Create VLANs
Modify VLANs
Delete VLANs
Advertise VLANs
```

Stores VLAN database locally.

---

## Client Mode

Capabilities:

```text
Receives VLAN database
Cannot create VLANs
```

---

### Analogy

```text
Read-only employee
```

Can see updates.

Cannot make changes.

---

## Transparent Mode

Capabilities:

```text
Does not synchronize
Maintains local VLAN database
Forwards advertisements
```

---

### Analogy

```text
Independent office
```

Passes messages through.

Does not obey updates.

---

# VTP Revision Number

This is the most important VTP concept.

---

## What Is It?

```text
Version number of the VLAN database
```

Every time a VLAN is:

```text
Added
Deleted
Modified
```

Revision number increases.

---

## Example

```text
Revision 5
```

Add VLAN 50.

Becomes:

```text
Revision 6
```

---

# Critical Rule

```text
Highest revision number wins
```

NOT:

```text
Newest switch
Production switch
Correct switch
```

Only:

```text
Highest revision number
```

---

# Dangerous Scenario

Current Network:

```text
VTP Domain = COMPANY
Revision = 5

VLAN 10
VLAN 20
VLAN 30
```

---

Old Lab Switch:

```text
VTP Domain = COMPANY
Revision = 50

VLAN 999
```

---

You connect the switch.

Network sees:

```text
50 > 5
```

Result:

```text
Entire VLAN database overwritten
```

Production VLANs disappear.

---

# Why VTP Is Dangerous

VTP assumes:

```text
Higher revision = newer information
```

It cannot determine:

```text
Which switch is correct
Which switch is production
Which switch is accidental
```

---

# Why Many Engineers Avoid VTP

Modern practice:

```text
Manually create VLANs
```

or

```text
Use VTP Transparent Mode
```

to avoid catastrophic mistakes.

---

# Resetting VTP Revision Number

Method 1:

```bash
vtp mode transparent
```

Then:

```bash
vtp mode server
```

Revision resets.

---

Method 2:

Temporarily change domain.

```bash
vtp domain TEMP
```

Then:

```bash
vtp domain COMPANY
```

---

# Troubleshooting

---

## Unexpected Trunk Formation

Cause:

```text
Dynamic desirable + dynamic auto
```

Result:

```text
Trunk formed automatically
```

---

## No Trunk Formation

Cause:

```text
Dynamic auto + dynamic auto
```

Result:

```text
No trunk
```

---

## VLANs Not Syncing

Check:

```text
Domain name
VTP mode
VTP version
Revision number
Trunk links
```

---

## VLAN Database Overwritten

Check:

```text
Recently connected switch
Revision number
Domain name
```

---

# Relevant Commands

---

## DTP

### View Port Status

```bash
show interfaces switchport
```

---

### Access Port

```bash
switchport mode access
```

---

### Trunk Port

```bash
switchport mode trunk
```

---

### Dynamic Desirable

```bash
switchport mode dynamic desirable
```

---

### Dynamic Auto

```bash
switchport mode dynamic auto
```

---

### Disable DTP

```bash
switchport nonegotiate
```

---

# VTP

### View Status

```bash
show vtp status
```

---

### Set Domain

```bash
vtp domain COMPANY
```

---

### Set Mode

```bash
vtp mode server
vtp mode client
vtp mode transparent
```

---

### Set Version

```bash
vtp version 2
```

---

# Key Takeaways

```text
DTP automatically negotiates trunk links.
```

```text
Dynamic desirable initiates trunking.
```

```text
Dynamic auto waits for another switch to initiate.
```

```text
Modern best practice is to manually configure trunks and disable DTP.
```

```text
VTP distributes VLAN databases between switches.
```

```text
VTP domains must match for synchronization to occur.
```

```text
Highest revision number wins.
```

```text
Server mode can modify VLANs.
Client mode cannot.
Transparent mode does not synchronize.
```

```text
Many enterprise networks avoid VTP because a single switch with a higher revision number can overwrite the entire VLAN database.
```

---

# Self-Check

1. What is the difference between Dynamic Auto and Dynamic Desirable?

2. Why does Auto + Auto not form a trunk?

3. What does `switchport nonegotiate` do?

4. What is a VTP domain?

5. Which VTP mode can create VLANs?

6. What is a VTP revision number?

7. What happens when a switch with a higher revision number joins the same VTP domain?

8. Why do many engineers prefer Transparent Mode or manual VLAN management today?
