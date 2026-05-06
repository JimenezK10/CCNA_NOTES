# Mock Enterprise IP Design (Clean + Practical)

We’ll use one big block:

```text
10.0.0.0/8
```

This is your **entire company**

---

# Step 1 — Break by Region

```text
10.0.0.0/8
```

Split into regions:

```text
10.1.0.0/16 → East Region
10.2.0.0/16 → West Region
10.3.0.0/16 → Central
```

---

# Step 2 — Break by Site / Building

Inside East:

```text
10.1.0.0/16
```

Split:

```text
10.1.1.0/24 → HQ Building
10.1.2.0/24 → Warehouse
10.1.3.0/24 → Remote Office
```

---

# Step 3 — Break by VLAN / Department

Inside HQ:

```text
10.1.1.0/24
```

Split:

```text
10.1.1.0/26   → IT Dept
10.1.1.64/26  → HR
10.1.1.128/26 → Finance
10.1.1.192/26 → Guest WiFi
```

---

# Step 4 — Router Links

Use small subnets:

```text
10.255.0.0/30 → Core ↔ Distribution
10.255.0.4/30 → Distribution ↔ Access
```

---

# Visual (Keep This in Your Head)

```text
10.0.0.0/8
   ↓
10.1.0.0/16 (Region)
   ↓
10.1.1.0/24 (Building)
   ↓
10.1.1.0/26 (Department)
   ↓
/30 links (routers)
```

---

# Why This Design Works

```text
Easy to scale
Easy to troubleshoot
Easy to summarize routes
```

Example:

```text
Router only needs:
10.1.0.0/16 → send to East
```

Doesn’t need every subnet

---

# Key Concepts You Just Used

* VLSM ✔
* Hierarchical addressing ✔
* Route summarization (intro) ✔

---

# Real-World Insight

Most enterprises do something like:

```text
10.<region>.<site>.<subnet>
```

Example:

```text
10.1.1.0 → East HQ
10.2.5.0 → West Site 5
```

---

# ⚡ Final Takeaway

```text
Big block → break into logical chunks → keep it organized
```
