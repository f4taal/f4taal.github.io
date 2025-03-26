---
date: "2025-03-26T00:00:00Z"
title: FireWall Testing and Evasion, Seed Project
tags : ["Firewall", "Network Security", "Linux", "VPN"]

---
# Introduction
**Bypassing Firewalls with VPN – A Hands-On Lab Experience**

In today’s digital landscape, understanding firewall evasion techniques is crucial for cybersecurity professionals, network administrators, and enthusiasts alike. While essential for securing networks, firewalls can sometimes be overly restrictive, blocking legitimate access to services or websites. [The SEED Labs Firewall Evasion Lab](https://seedsecuritylabs.org/Labs_20.04/Networking/Firewall_Evasion/), designed by Wenliang Du, provides a hands-on approach to mastering tunneling techniques such as VPN and port forwarding to bypass these restrictions. In this blog, we’ll walk you through the lab step-by-step, explain the network setup, and provide detailed instructions optimized for SEO with keywords like firewall evasion, SSH tunneling, VPN bypass, and port forwarding tutorial. Let’s dive in!

## Understanding the Lab’s Network Setup

![image](/img/firewall/firewal-labsetup.png)

To effectively tackle the SEED Labs Firewall Evasion Lab, it’s essential first to grasp the intricacies of its network architecture, as illustrated in Figure 1 of the lab document. The setup is divided into two primary segments: the external network (10.8.0.0/24) and the internal network (192.168.20.0/24), connected via a dual-interface router/firewall. The external network, simulating the broader “outside world” or public internet, includes key hosts such as 10.8.0.1, the host machine (typically your virtual machine), which serves as the gateway to the Internet, and 10.8.0.11, the router’s external-facing interface. Additional external hosts—A (10.8.0.99), A1 (10.8.0.5), and A2 (10.8.0.6)—are designated for testing evasion techniques. Conversely, the internal network represents a private, restricted environment shielded by the firewall, featuring hosts like B (192.168.20.99), B1 (192.168.20.5), and B2 (192.168.20.6), all tethered to the router’s internal interface at 192.168.20.11. The router/firewall, bridging these networks, enforces ingress filtering—allowing only SSH traffic into the internal network—and egress filtering, which blocks access to specific external websites. Traffic from both networks flows through 10.8.0.1 to reach the Internet, with the router employing Network Address Translation (NAT) to conceal internal IP addresses during outbound communication. This configuration mirrors real-world enterprise or institutional networks where stringent firewall policies limit connectivity, providing an ideal sandbox for exploring and mastering firewall evasion strategies.

# Task 0: Setting Up and Verifying the Environment
### Objective
Familiarize yourself with the lab environment and enhance the firewall rules.

## Step 1: Download the Lab Files
2. Get the pre-built Ubuntu 20.04 VM and container setup files from the SEED Labs website.
3. Use Docker to deploy the network topology.

![image](/img/firewall/firewall-1.png)

![image](/img/firewall/firewall-7.png)

## Step 2: Verify Network Interfaces on the router container

![image](/img/firewall/firewall-0.png)

Ensure eth0 connects to 10.8.0.0/24 and eth1 to 192.168.20.0/24. Adjust docker-compose.yml if needed.

## Step 3: Enhance Firewall Rules
1. Block two additional websites (e.g., bbc.com and ).
2. Find their IP ranges (use nslookup or web search):
3. Add rules to docker-compose.yml

```bash
iptables -A FORWARD -i eth1 -d 157.240.0.0/16 -j DROP
iptables -A FORWARD -i eth1 -d 104.244.42.0/24 -j DROP
```
![image](/img/firewall/firewall-8.png)

![image](/img/firewall/firewall-2.png)

4. Restart containers

```bash
docker-compose up --build
```

5. Verify Rules
From an internal host (e.g., B: 192.168.20.99), test:

```bash
curl www.example.com
curl www.facebook.com
curl www.twitter.com
```
![image](/img/firewall/firewall-9.png)

All three requests failed due to egress filtering.

*The firewall’s ingress rules allow only SSH traffic ```--dport 22```, while egress rules block specific IP ranges. Adding more blocked sites prepares us for testing evasion techniques.*


# Task 1: Static Port Forwarding
Use SSH static port forwarding to bypass the ingress firewall and access a restricted service (e.g., Telnet) on the internal network. Create a tunnel between the external and internal networks to connect the telnet to the server on B1. We should telnet from hosts A, A1 and A2. 

*Q1: How many TCP connections are involved in this entire process. You should run wireshark or tcpdump to capture the network traffic, and then point out all the involved TCP connections from the captured traffic*.

![image](/img/firewall/firewall-10.png)

## Set Up the Tunnel
From host A (10.8.0.99), create a tunnel to B (192.168.20.99).

```bash
ssh -4NT -L 0.0.0.0:4444:192.168.20.5:23 root@192.168.20.99
```
![image](/img/firewall/firewall-11.png)

0.0.0.0:4444: Listen on all interfaces of A, port 4444.
192.168.20.5:23: Forward to B1’s Telnet port.

## Test the Tunnel
From A, A1, and A2, run:

```bash
telnet 10.8.0.5 2023
```

![image](/img/firewall/firewallstatic-1.png)

Connects to B1’s Telnet server.

## Analyze Traffic
Run tcpdump on the router

```bash
tcpdump -i eth0
``` 

Observe two TCP connections
A (10.8.0.99:random_port) → B (192.168.20.99:22) for SSH.
B (192.168.20.99:random_port) → B1 (192.168.20.5:23) for Telnet.

*Q2: Why can this tunnel successfully help users evade the firewall rule specified in the lab setup?*
The firewall allows SSH traffic (port 22). The tunnel encapsulates Telnet data within SSH, evading the ingress restriction.


# Task 2: Dynamic Port Forwarding
Use dynamic port forwarding to access multiple blocked websites from the internal network using curl from hosts B, B1, and B2.

### Task 2.1: Setting Up Dynamic *firewall bypass*
Create the Tunnel:
From B (192.168.20.99):
```bash
ssh -4NT -D 0.0.0.0:1080 user@10.8.0.99
```
![image](/img/firewall/firewall-14.png)

B becomes a SOCKS5 proxy.
Test with curl from B, B1, and B2:

```bash
curl --proxy socks5h://192.168.20.99:1080 http://www.example.com/
```
![image](/img/firewall/firewall-15.png)

```bash
curl --proxy socks5h://192.168.20.99:1080 https://www.bbc.com/
```
![image](/img/firewall/firewall-16.png)

```bash
curl -I --proxy socks5h://192.168.20.99:1080 https://edition.cnn.com/
```
![image](/img/firewall/firewall-17.png)

All requests succeed.

*Q1: Which computer establishes the actual connection with the intended web server?* **Host A (10.8.0.99) establishes the connection to the web server.**
*Q2: How does this computer know which server it should connect to?* **The SOCKS5 protocol negotiates the target address dynamically.**


## Task 2.2: Browser Testing
Configure Firefox:
Go to 
```about:preferences → Network Settings → Manual Proxy → SOCKS Host: 192.168.20.99, Port: 1080.```

![image](/img/firewall/firewall-18.png)

### Test Browsing
Visit blocked sites from the host VM (192.168.20.1).

![image](/img/firewall/firewall-19.png)

![image](/img/firewall/firewall-20.png)

![image](/img/firewall/firewall-21.png)

Use tcpdump on the router to confirm tunnel traffic.

![image](/img/firewall/firewall-22.png)

![image](/img/firewall/firewall-23.png)

Break the Tunnel by killing the SSH process on B and when you retry browsing the requests should fail.

## Task 2.3: SOCKS Client in Python

In this lab task, write a python script and use it to access http://www.example.com from hosts B, B1, and B2. The code should send HTTP requests, not HTTPS requests (sending HTTPS requests is much more complicated due to the TLS handshake). Run on B, B1, B2:

``` Python
#!/bin/env python3

import socks

s = socks.socksocket()

# Set the proxy
s.set_proxy(socks.SOCKS5, "192.168.20.99", 9000) 

# Connect to final destination via the proxy
hostname = "www.example.com"
s.connect((hostname, 80))

request = b"GET / HTTP/1.0\r\nHost: " + hostname.encode('utf-8') + b"\r\n\r\n"
s.sendall(request)

# Get the response
response = s.recv(2048)
while response:
  print(response.split(b"\r\n"))
  response = s.recv(2048)
```

![image](/img/firewall/firewall-24.png)

There is HTTP response from www.example.com because one tunnel serves multiple destinations via SOCKS5(Dynamic Advantage)

# Task 3: VPN Tunneling
Create a VPN tunnel between A and B, with B being the VPN server. Then conduct all the necessary configuration. Once everything is set up try to telnet to B, B1, and B2 from the outside network then capture the packets trace, and explain why the packets are not blocked by the firewall.

## Task 3.1: Bypassing Ingress Firewall
1. Create VPN from A to B

```bash
ssh -w 0:0 root@192.168.20.99 \
-o "PermitLocalCommand=yes" \
-o "LocalCommand=ip addr add 192.168.53.88/24 dev tun0 && ip link set tun0 up" \
-o "RemoteCommand=ip addr add 192.168.53.99/24 dev tun0 && ip link set tun0 up"
```
![image](/img/firewall/firewall-25.png)

2. Configure Routing on A: ```ip route add 192.168.20.0/24 dev tun0``` the test telnet from A: ```telnet 192.168.20.5 23```

## Task 3.2: Bypassing Egress Firewall
Create VPN from B to A the set Up NAT on A the test access from B, B1, and B2 using curl to access blocked sites.
After this set up, we were able to access the blocked sites, this is because VPN encrypts traffic, making it indistinguishable from SSH, bypassing both ingress and egress rules.

![image](/img/firewall/firewall-26.png)

![image](/img/firewall/firewall-27.png)


# Task 4: SOCKS5 vs. VPN Comparison

| **Aspect**         | **SOCKS5 Proxy**                  | **VPN**                         |
|---------------------|------------------------------------|----------------------------------|
| **Layer**          | Application layer                 | Network layer                   |
| **Setup**          | App-specific configuration        | System-wide routing             |
| **Flexibility**    | Per-app tunneling                 | Entire network tunneling        |
| **Pros**           | Lightweight, app-level control    | Comprehensive, secure           |
| **Cons**           | Limited to supported apps         | More complex setup              |

# Conclusion
The SEED Labs Firewall Evasion Lab offers a deep dive into firewall evasion techniques, from static port forwarding to VPN tunneling. By mastering these skills, you’ll be equipped to navigate restrictive networks effectively. Experiment, analyze, and document your findings for a thorough understanding!





