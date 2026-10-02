# Module 1 --- Firewall Fundamentals & ICMP Filtering

## Overview

This module covers the first hands-on OPNsense firewall exercise:

-   Firewall fundamentals
-   Firewall rule creation
-   Rule ordering
-   ICMP blocking
-   Connectivity testing
-   Firewall log analysis
-   Rule rollback

> **Security:** Internal IP addresses are masked as `192.168.x.x` for
> public documentation.

------------------------------------------------------------------------

## 1. Firewall Basics

A firewall evaluates network traffic and applies security rules.

``` text
Client
  |
  v
OPNsense
  |
  v
Firewall Rule
  |
  +--> PASS
  |
  +--> BLOCK
  |
  +--> REJECT
```

Important rule fields include:

-   Source
-   Destination
-   Protocol
-   Port
-   Interface
-   Action

------------------------------------------------------------------------

## 2. Baseline Testing

Before changing the firewall, connectivity was tested.

### Ping

``` bash
ping -c 4 192.168.x.x
```

### Internet Connectivity

``` bash
ping -c 4 8.8.8.8
```

### DNS

``` bash
ping -c 4 google.com
```

### HTTPS

``` bash
curl -I https://example.com
```

These tests established the initial network state.

------------------------------------------------------------------------

## 3. Inspect Existing Firewall Rule

The LAN firewall rules were inspected in:

``` text
Firewall → Rules → LAN
```

A broad default PASS rule was present:

``` text
Action      : PASS
Protocol    : Any
Source      : LAN network
Destination : Any
```

------------------------------------------------------------------------

## 4. ICMP BLOCK Rule

A temporary ICMP blocking rule was created:

``` text
Action      : BLOCK
Interface   : LAN
Protocol    : ICMP
Source      : LAN network
Destination : Any
Description : LAB-block ICMP from LAN
```

### Rule Order

Initially:

``` text
1. PASS  LAN → Any
2. BLOCK LAN → Any (ICMP)
```

Ping still worked because the broad PASS rule matched first.

The order was changed to:

``` text
1. BLOCK LAN → Any (ICMP)
2. PASS  LAN → Any
```

After this change, ICMP traffic was blocked.

------------------------------------------------------------------------

## 5. ICMP Testing

``` bash
ping -c 4 192.168.x.x
```

Packet flow:

``` text
Client
  |
  | ICMP
  v
OPNsense
  |
  v
BLOCK Rule
  |
  v
DROP
```

This confirmed that the firewall rule was working.

------------------------------------------------------------------------

## 6. HTTPS Test

After blocking ICMP, HTTPS was tested:

``` bash
curl -I https://example.com
```

HTTPS continued to work.

This demonstrated that blocking one protocol does not automatically
block unrelated traffic.

------------------------------------------------------------------------

## 7. Firewall Log Analysis

Logging was enabled on the ICMP BLOCK rule.

The blocked traffic was analysed through:

``` text
Firewall → Log Files → Live View
```

The log was used to verify:

``` text
Source IP
Destination IP
Protocol
Interface
Action
Timestamp
```

The Live View confirmed that ICMP traffic was being blocked by the
configured rule.

------------------------------------------------------------------------

## 8. Rollback

After testing, the temporary ICMP BLOCK rule was disabled.

Connectivity was verified again using:

``` bash
ping -c 4 192.168.x.x
curl -I https://example.com
```

This confirmed that the temporary firewall change had been successfully
rolled back.

------------------------------------------------------------------------

## Key Learnings

-   Firewall rules control network traffic.
-   Rule order matters.
-   Specific rules should be placed before broad rules when required.
-   ICMP can be filtered independently.
-   `curl` can be used to verify HTTPS connectivity.
-   Firewall logs help investigate blocked traffic.
-   Temporary lab rules should be rolled back after testing.

------------------------------------------------------------------------

