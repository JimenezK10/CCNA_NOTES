# Connecting to Cisco Devices & CLI Basics (Refined + Visual)

---

## Console Access (Local Connection)

To access a Cisco device locally, connect your computer to the **console port**.

### Console Port Types

* RJ-45
* USB Mini-B
* USB-C (newer devices)

### Required Hardware

* **Rollover cable** (for RJ-45)
* **USB-to-serial adapter** (modern laptops)

> This is a **direct physical connection** — no network required.

---

## Terminal Emulator Setup (PuTTY)

You need a terminal emulator like PuTTY.

### Default Cisco Serial Settings

* Connection: **Serial**
* Port: **COMx**
* Speed: **9600**
* Data bits: **8**
* Stop bits: **1**
* Parity: **None**
* Flow control: **None**

> These defaults are **exam critical**.

---

## Initial CLI Access

When prompted:

```
Would you like to enter the initial configuration dialog?
```

Type:

```
no
```

Press **Enter** to continue.

---

# CLI Modes (Core Concept)

## Mode Hierarchy

```text
User EXEC (>)
    ↓ enable
Privileged EXEC (#)
    ↓ configure terminal
Global Config (config)#
```

---

## User EXEC Mode

```
Router>
Switch>
```

### Characteristics

* Limited access
* View-only commands
* No configuration changes

---

## Privileged EXEC Mode

Enter:

```
enable
```

Prompt:

```
Router#
```

### Capabilities

* View full configuration
* Save config
* Reload device
* Debug / troubleshoot

> Still cannot directly configure—must enter config mode

---

## Global Configuration Mode

Enter:

```
configure terminal
```

Prompt:

```
Router(config)#
```

### Purpose

* Make configuration changes
* Modify device behavior

---

# CLI Efficiency Tools

* `?` → command help
* `Tab` → autocomplete
* Partial commands allowed (if unambiguous)

Examples:

```
en  → enable
ex  → exit
```

---

# Device Identity

Set hostname (in config mode):

* Appears in prompt
* Helps identify devices in networks

---

# Security Layers (CRITICAL CONCEPT)

## Full Access Flow

```text
[MOTD Banner]
        ↓
Console Password Prompt
        ↓
Router>   (User EXEC)
        ↓ enable
Enable Secret Prompt
        ↓
Router#   (Privileged EXEC)
```

---

## 1. MOTD Banner (Message of the Day)



command: banner motd # Unauthorized access prohibited #


The `#` is a **delimiter**. It marks where the banner message starts and ends.

Example flow:

```text
banner motd # This system is for authorized users only #
```

You can use another delimiter too:

```text
banner motd $ Authorized access only $
```

Just don’t use that delimiter inside the message itself.


### Purpose

* Displays **before login**
* Legal / warning message

Example:

```
*** Unauthorized access prohibited ***
```

### Key Point

* Adds **zero security**
* Only informational

> Think: “Warning sign on the door”

---

## 2. Console Line Security (Physical Access Control)

### Concept

Protects access through the **console port**

### What it does

* Requires password before entering CLI

### Key Requirement

* Password must be set
* Login must be enforced

### Behavior

**Before:**

```
Connect → Router>
```

**After:**

```
Connect → Password → Router>
```

### Critical Rule

> Password without login = useless

---

## 3. Privileged EXEC Security

### Enable Password (Weak)

* Stored in plaintext (or weak encryption)
* Visible in config
* Not secure

---

### Enable Secret (Strong)

* Uses **MD5 hash (Type 5)**
* Hidden in config
* Overrides enable password

### Example:

```
enable secret 5 <hash>
```

---

### Exam Rule

> **Enable Secret ALWAYS takes precedence over Enable Password**

---

# Password Encryption

## Service Password Encryption

Command:

```
service password-encryption
```

### Effect

* Encrypts passwords using **Type 7**

### Reality

* Easily reversible
* Not truly secure

---

## Behavior Summary

| Feature                     | Affects           | Security Level |
| --------------------------- | ----------------- | -------------- |
| enable password             | Privileged access | Weak           |
| enable secret               | Privileged access | Strong         |
| service password-encryption | Line passwords    | Weak           |
| console password            | Physical access   | Basic          |

---

# Configuration Files

## Running Configuration

* Active config (RAM)
* Changes take effect immediately

View:

```
show running-config
```

---

## Startup Configuration

* Saved config (NVRAM)
* Loaded on reboot

View:

```
show startup-config
```

---

## Saving Configuration

All from Privileged EXEC:

```
write
write memory
copy running-config startup-config
```

### Key Concept

> If you don’t save → config is lost on reboot

---

# Using EXEC Commands in Config Mode

Run EXEC commands inside config mode:

```
do show running-config
```

---

# Removing Commands

Use `no` to negate:

```
no service password-encryption
```

---

# Packet Tracer Notes

Cisco Packet Tracer

* Network simulator
* Great for CCNA practice
* Limited command set vs real devices

---

# Final Mental Model (What You Should Retain)

You are configuring **layers of access control**:

1. **MOTD** → Warning
2. **Console Password** → Entry to CLI
3. **Enable Secret** → Elevated access
4. **Config Save** → Persistence

---

# Mentor Notes (Blunt but Important)

* You now understand **how to access and secure a device**
* You are still **not doing networking yet**
* That’s fine—but don’t stay here too long

Your foundation is getting solid.

---

If you want next step (still controlled, not rushing):
I’ll give you **3 broken configs** and you diagnose them

That’s where people either become sharp—or stay average.
