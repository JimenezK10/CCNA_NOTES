# Subnetting — Part 1 (Extended)

---

## Purpose

```text
Efficient IP usage
```

Avoid wasting addresses on links and small networks.

---

## Key Idea

```text
Longer prefix = smaller network
Shorter prefix = larger network
```

---

## CIDR

* Removes class restrictions
* Any prefix length is valid

```text
/8 /16 /24 ❌ not required
/25 /26 /27 /30 ✔ valid
```

---

## Router Rule

```text
Each router interface = one network
```

---

## Usable IP Formula

[
2^{host\ bits} - 2
]

* Network address
* Broadcast address

---

## Host Counts Table

| Prefix | Host Bits | Usable IPs | Subnet Mask     |
| ------ | --------- | ---------- | --------------- |
| /25    | 7         | 126        | 255.255.255.128 |
| /26    | 6         | 62         | 255.255.255.192 |
| /27    | 5         | 30         | 255.255.255.224 |
| /28    | 4         | 14         | 255.255.255.240 |
| /29    | 3         | 6          | 255.255.255.248 |
| /30    | 2         | 2          | 255.255.255.252 |

---

## /30 Subnet

Used for:

```text
Router-to-router links
```

Example:

```text
Network   → .0
Host      → .1
Host      → .2
Broadcast → .3
```

---

## /31

```text
2 usable IPs
No broadcast
```

---

## /32

```text
Single host
```

---

## Block Size Rule

```text
Block size = 256 - subnet mask (interesting octet)
```

Example:

```text
/26 → 255.255.255.192
256 - 192 = 64
```

Subnets:

```text
0, 64, 128, 192
```

---

# 🔥 Subnet Cheat Sheet (Must Know)

## Common Masks

```text
/24 → 255.255.255.0   → block 256
/25 → 255.255.255.128 → block 128
/26 → 255.255.255.192 → block 64
/27 → 255.255.255.224 → block 32
/28 → 255.255.255.240 → block 16
/29 → 255.255.255.248 → block 8
/30 → 255.255.255.252 → block 4
```

---

## Pattern to Memorize

```text
128 192 224 240 248 252
```

That’s the progression.

---

## Subnet Boundaries

| Prefix | Range Size |
| ------ | ---------- |
| /25    | 128        |
| /26    | 64         |
| /27    | 32         |
| /28    | 16         |
| /29    | 8          |
| /30    | 4          |

---

## Fast Mental Method

```text
Find mask →
Find block size →
Count up in that increment →
That’s your subnets
```

---

# 🔥 Wildcard Masks (New)

Wildcard mask = **inverse of subnet mask**

```text
Subnet mask:   255.255.255.224
Wildcard mask: 0.0.0.31
```

---

## How to Calculate

```text
255 - subnet mask octet
```

Example:

```text
255.255.255.192
→ wildcard = 0.0.0.63
```

---

## Purpose

Used in:

* ACLs
* Routing protocols (later)

---

## Meaning

```text
0 = must match exactly
1 = ignore this bit
```

---

## Examples

```text
Subnet:   192.168.1.0/27
Mask:     255.255.255.224
Wildcard: 0.0.0.31
```

---

## Practical Use (ACL Example)

```bash
access-list 1 permit 192.168.1.0 0.0.0.31
```

Matches:

```text
192.168.1.0 → 192.168.1.31
```

---

## Key Rule

```text
Subnet mask = defines network
Wildcard mask = defines match range
```

---

# 🔥 Final Mental Anchors

```text
Subnetting = borrowing host bits
```

```text
Block size = subnet spacing
```

```text
Wildcard = inverse mask
```

```text
Stay inside subnet boundaries
```

---

## Self-Check

1. What is block size for /27?
   → 32

2. Wildcard for /26?
   → 0.0.0.63

3. Why use /30?
   → 2 usable IPs

4. What does wildcard 0 mean?
   → Must match exactly
