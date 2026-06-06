# Networking Task 01 — Understanding Your Network Environment

**Author:** Sanket
**Objective:** Understand the basic components of a network and identify the network configuration of my own device.

---

## Part A: Network Information

> Values below are from my own system (macOS). Gathered using `scutil`, `ipconfig getifaddr`, `ifconfig`, `route`, and `scutil --dns`. Screenshots are in the `/screenshots` folder.

| # | Item | Value |
|---|------|-------|
| 1 | Hostname (Device Name) | Sanket's MacBook Pro (`Sankets-MacBook-Pro.local`) |
| 2 | IPv4 Address | `192.168.1.8` |
| 3 | MAC Address | `1:1:1:1:1:1:1` |
| 4 | Default Gateway | `192.168.1.1` |
| 5 | DNS Server | `192.168.1.1` (router relay) + ISP IPv6 DNS: `2401:4900:50:9::7ad`, `2401:4900:50:9::7b5` |

**Active interface:** `en7` — this is a wired/USB-Ethernet (or dock) connection rather than Wi-Fi (`en0`). The router at `192.168.1.1` is both the default gateway and the local DNS forwarder, relaying queries to the ISP's (Airtel) IPv6 DNS servers.

---

## Part B: Basic Networking Concepts

**What is an IP Address?**
An IP (Internet Protocol) address is a unique numerical label assigned to each device on a network. It serves two jobs: identifying the device and providing its location so data can be routed to and from it. It works like a postal address — without it, the network wouldn't know where to deliver packets. The common format is IPv4 (e.g. `192.168.1.12`), with IPv6 used for the larger modern address space.

**What is a MAC Address?**
A MAC (Media Access Control) address is a hardware identifier permanently assigned to a device's network interface card (NIC) by the manufacturer. It's a 48-bit value written as six hexadecimal pairs (e.g. `A4:B1:C2:D3:E4:F5`). It operates at Layer 2 (Data Link) and is used to deliver frames between devices on the *same local network*. Unlike an IP address, it normally doesn't change and isn't routable across the internet.

**What is a Default Gateway?**
The default gateway is the device (usually your router) that traffic is sent to whenever the destination lies *outside* your local subnet. It's the "exit door" from your LAN to other networks and the internet. On a home network it's typically the router's LAN IP, such as `192.168.1.1`. If your machine can't reach the gateway, it can't reach anything beyond the local network.

**What is DNS?**
DNS (Domain Name System) translates human-friendly domain names like `google.com` into the IP addresses machines actually use to connect. It acts as the internet's phonebook: you type a name, DNS returns the corresponding IP, and your device then connects to that address. Without DNS you'd have to memorise raw IP addresses for every site.

**Difference between Public IP and Private IP**

| | Private IP | Public IP |
|---|-----------|-----------|
| Scope | Used inside a local network (LAN) | Globally unique on the internet |
| Routable on internet? | No | Yes |
| Assigned by | Router (via DHCP) | ISP |
| Ranges | `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16` | Everything outside private/reserved ranges |
| Example | `192.168.1.12` | `49.36.x.x` (whatever your ISP gives) |

Multiple devices on a home network share a single public IP via **NAT** (Network Address Translation), which the router performs by mapping private addresses to the public one.

---

## Part C: Network Diagram

See `network_diagram.svg` (or `network_diagram.png`) in this folder.

```
        Internet  (Public IP, ISP — Airtel)
            |
            v
     Router / Gateway   (192.168.1.1 — does NAT + DHCP + DNS relay)
            |
            v (wired / en7)
      My MacBook Pro   (Private IP: 192.168.1.8, MAC: 9c:69:d3:79:13:f1)
```

---

## Part D: Network Connectivity Test

> Commands run on my system (macOS). Full output screenshots are in `/screenshots`.

**macOS (mine):** `ifconfig` · `netstat -nr | grep default` · `scutil --dns` · `ping -c 4 google.com` · `traceroute google.com`
**Windows:** `ipconfig` · `ping google.com` · `tracert google.com`
**Linux:** `ip addr` (or `ifconfig`) · `ping google.com` · `traceroute google.com`

**Answers:**

1. **Was the ping successful?**
   Yes. 4 packets transmitted, 4 received, **0% packet loss**. Round-trip time averaged **28.9 ms** (min 28.0 / max 29.7 ms), with a TTL of 114 — a healthy, stable connection to `google.com` (142.250.134.113).

2. **How many hops were shown?**
   The path reached the destination at **hop 19** (`fx-in-f113.1e100.net`, 142.250.134.113). Several intermediate hops (5, 6, 9, and 13–18) returned `*` instead of a reply — this is normal, as some routers are configured not to respond to traceroute probes — but the trace completed successfully. The first hop is my router (192.168.1.1), followed by Airtel's network, then Google's edge.

3. **What is the purpose of traceroute?**
   Traceroute maps the path a packet takes from my device to a destination, listing every intermediate router (hop) along the way together with the latency to each. It's a diagnostic tool used to see *where* in the path a connection slows down or fails, which helps locate the source of a network problem.

---

## Folder Contents

```
Networking_Task_01_Sanket/
├── README.md              <- this file
├── network_diagram.svg    <- Part C diagram
├── screenshots/
│   ├── ipconfig.png
│   ├── ping.png
│   └── tracert.png
└── command_outputs.txt    <- (optional) raw text output of the commands
```
# Networking-Task-01-network-environment-writeup
