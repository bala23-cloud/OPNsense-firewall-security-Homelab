# Module 2 — Firewall Rule Matching & Rule Order

## Overview

This module focused on understanding how OPNsense evaluates firewall rules and how rule order, source, destination, and protocol affect traffic decisions.

## What I Practiced

### 1. Rule Matching Concept

Learned how OPNsense compares incoming traffic against firewall rules.

```text
Packet
  ↓
Rule Matching
  ↓
PASS / BLOCK
```

### 2. Specific vs Broad Rules

Tested the difference between:

- Specific rules — ICMP, specific source/destination
- Broad rules — LAN → Any

Learned that broad rules can match traffic before more specific rules if placed above them.

### 3. Rule Order Experiment

Tested rule ordering:

```text
BLOCK ICMP
     ↓
PASS LAN → Any
```

vs.

```text
PASS LAN → Any
     ↓
BLOCK ICMP
```

Observed that the first matching rule determines the firewall decision.

### 4. PASS + BLOCK Conflict Test

Created conflicting PASS and BLOCK rules and tested the same traffic.

```text
Packet
  ↓
First matching rule
  ↓
Decision
```

This demonstrated the importance of correct rule ordering.

### 5. Source / Destination Matching

Created rules using:

- Specific source host
- Specific destination host
- ICMP protocol

Tested how traffic is allowed or blocked based on both source and destination.

### 6. Logs Verification

Enabled logging for test rules and verified blocked traffic using:

**Firewall → Log Files → Live View**

Checked:

- Source
- Destination
- Protocol
- Action
- Timestamp

### Screenshot — OPNsense Firewall Live View

![OPNsense Firewall Live View](Module_2_Log_View.png)
