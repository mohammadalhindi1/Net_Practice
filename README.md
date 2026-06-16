# NetPractice - 42 Network Configuration Project

<p align="center">
  <img src="screenshots/level10.png" alt="NetPractice Preview" width="900"/>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/42-NetPractice-blue" alt="42 NetPractice"/>
  <img src="https://img.shields.io/badge/Topic-Networking-green" alt="Networking"/>
  <img src="https://img.shields.io/badge/Status-Completed-brightgreen" alt="Completed"/>
</p>

## Overview

This repository contains my solutions, screenshots, and explanations for the **NetPractice** project from the **42 curriculum**.

NetPractice is a practical networking project where the goal is to configure IP addresses, subnet masks, gateways, and routing tables so that hosts can communicate correctly across different networks.

The main goal of this repository is not only to store the final configuration files, but also to explain the reasoning behind each level.

---

## Contents

* [Project Structure](#project-structure)
* [Core Networking Concepts](#core-networking-concepts)
* [How I Solve NetPractice Levels](#how-i-solve-netpractice-levels)
* [Subnet Planning Cheat Sheet](#subnet-planning-cheat-sheet)
* [Level Walkthroughs](#level-walkthroughs)
* [Common NetPractice Errors](#common-netpractice-errors)
* [42 Note](#42-note)
* [Author](#author)

---

## Project Structure

```text
.
├── README.md
├── configs/
│   ├── level1.json
│   ├── level2.json
│   ├── level3.json
│   ├── level4.json
│   ├── level5.json
│   ├── level6.json
│   ├── level7.json
│   ├── level8.json
│   ├── level9.json
│   └── level10.json
└── screenshots/
    ├── level1.png
    ├── level2.png
    ├── level3.png
    ├── level4.png
    ├── level5.png
    ├── level6.png
    ├── level7.png
    ├── level8.png
    ├── level9.png
    └── level10.png
```

| Folder         | Description                                                |
| -------------- | ---------------------------------------------------------- |
| `configs/`     | Contains the saved JSON configuration files for each level |
| `screenshots/` | Contains screenshots of the solved levels                  |
| `README.md`    | Contains the full project explanation and walkthrough      |

---

## Core Networking Concepts

### IP Address

An IP address identifies an interface inside a network.

Example:

```text
192.168.1.10
```

In NetPractice, every interface must have a valid IP address that belongs to the correct subnet.

---

### Subnet Mask

A subnet mask defines the size of the network.

Example:

```text
255.255.255.0
```

This is the same as:

```text
/24
```

The subnet mask tells us which part of the IP address represents the network and which part represents the hosts.

---

### Network Address

The network address represents the subnet itself.

Example:

```text
192.168.1.0/24
```

Here:

```text
192.168.1.0
```

is the network address.

It cannot be assigned to a device.

---

### Broadcast Address

The broadcast address is the last address in a subnet.

Example:

```text
192.168.1.255
```

for:

```text
192.168.1.0/24
```

It is used to send traffic to all devices in the subnet.

It cannot be assigned to a device.

---

### Usable Host Range

The usable host range contains the IP addresses that can be assigned to devices.

Example:

```text
192.168.1.0/24
```

Usable IP range:

```text
192.168.1.1 - 192.168.1.254
```

---

### Gateway

A gateway is the next hop used to reach another network.

Important rule:

```text
The gateway must be directly reachable.
```

That means the gateway IP must be in the same subnet as the interface sending the packet.

Example:

```text
Host:    192.168.1.10/24
Gateway: 192.168.1.1
```

This is valid because both addresses are in:

```text
192.168.1.0/24
```

---

### Routing Table

A routing table tells a device where to send traffic.

A route has two sides:

```text
Destination => Gateway
```

Example:

```text
10.0.2.0/24 => 10.0.1.2
```

Meaning:

```text
To reach 10.0.2.0/24, send the packet to 10.0.1.2.
```

---

### Default Route

A default route is used when no more specific route matches.

It can be written as:

```text
0.0.0.0/0
```

or sometimes:

```text
default
```

It means:

```text
Any destination.
```

Example:

```text
0.0.0.0/0 => 192.168.1.1
```

---

### Switch

A switch connects devices inside the same network.

A switch does not separate networks.

If devices are connected to the same switch, they usually need to be in the same subnet.

---

### Router

A router connects different networks.

Each router interface usually belongs to a different subnet.

Routers need routes to reach networks that are not directly connected to them.

---

## How I Solve NetPractice Levels

My method for solving NetPractice is:

1. Read the goal carefully.
2. Identify which hosts need to communicate.
3. Split the diagram into separate networks.
4. Check which devices are directly connected.
5. Make sure directly connected interfaces are in the same subnet.
6. Avoid using network or broadcast addresses.
7. Set host gateways to the nearest router interface.
8. Add routes on routers for remote networks.
9. Make sure the forward path works.
10. Make sure the reverse path works.
11. Read the logs and fix the first real error.

The most important idea is:

```text
Do not guess. Split the topology into small networks first.
```

---

## Subnet Planning Cheat Sheet

| Required Usable IPs | Best Subnet |       Subnet Mask |
| ------------------: | ----------: | ----------------: |
|                   2 |       `/30` | `255.255.255.252` |
|               3 - 6 |       `/29` | `255.255.255.248` |
|              7 - 14 |       `/28` | `255.255.255.240` |
|             15 - 30 |       `/27` | `255.255.255.224` |
|             31 - 62 |       `/26` | `255.255.255.192` |
|            63 - 126 |       `/25` | `255.255.255.128` |
|           127 - 254 |       `/24` |   `255.255.255.0` |

### Quick Rule

For a direct link between two devices:

```text
/30 is usually enough.
```

For a switch with multiple devices:

```text
Use a subnet large enough for all connected devices.
```

---

## Level Walkthroughs

---

<details>
<summary><strong>Level 1 - Direct Host-to-Host Communication</strong></summary>

<br>

<img src="screenshots/level1.png" alt="Level 1" width="900"/>

### Goal

This level contains two direct communication goals:

* Host A needs to communicate with Host B.
* Host C needs to communicate with Host D.

There are no routers in this level.

---

### Network Analysis

The topology has two separate direct connections:

```text
A <--> B

C <--> D
```

That means:

* `A1` and `B1` must be in the same subnet.
* `C1` and `D1` must be in the same subnet.

Since there are no routers, no gateway is required.

---

### Final Configuration

| Interface |       IP Address |     Subnet Mask |
| --------- | ---------------: | --------------: |
| A1        |   `104.95.23.11` | `255.255.255.0` |
| B1        |   `104.95.23.12` | `255.255.255.0` |
| C1        | `211.191.135.75` |   `255.255.0.0` |
| D1        | `211.191.135.74` |   `255.255.0.0` |

---

### Why It Works

For A and B:

```text
A1 = 104.95.23.11/24
B1 = 104.95.23.12/24
```

Both are inside:

```text
104.95.23.0/24
```

For C and D:

```text
C1 = 211.191.135.75/16
D1 = 211.191.135.74/16
```

Both are inside:

```text
211.191.0.0/16
```

Because each pair is directly connected and belongs to the same subnet, communication works without a router.

---

### Key Takeaway

Directly connected devices must be in the same subnet.

No gateway is needed when there is no router.

---

### Configuration File

[`configs/level1.json`](configs/level1.json)

</details>

---

<details>
<summary><strong>Level 2 - Basic Subnet Matching</strong></summary>

<br>

<img src="screenshots/level2.png" alt="Level 2" width="900"/>

### Goal

The goal of this level is to make directly connected hosts communicate by correcting their IP addresses and subnet masks.

---

### Network Analysis

This level focuses on checking whether two connected interfaces belong to the same subnet.

When two hosts are connected directly, they do not need a gateway.

They only need:

```text
Valid IP addresses inside the same subnet.
```

---

### Solving Strategy

For every connected pair:

1. Look at the IP address.
2. Look at the subnet mask.
3. Calculate the network address.
4. Make sure both interfaces are inside the same network.
5. Make sure neither IP is a network address or broadcast address.

---

### Why It Works

The communication works when both interfaces agree on the same network range.

If one device thinks the other device is outside the subnet, it will try to use a route or gateway, which is not needed in a direct connection.

---

### Key Takeaway

The IP address alone is not enough.

You always need:

```text
IP address + subnet mask
```

to know whether two devices are in the same network.

---

### Configuration File

[`configs/level2.json`](configs/level2.json)

</details>

---

<details>
<summary><strong>Level 3 - Gateway Basics</strong></summary>

<br>

<img src="screenshots/level3.png" alt="Level 3" width="900"/>

### Goal

This level introduces the idea of a gateway.

The host needs to communicate with another network through a router.

---

### Network Analysis

When a host wants to reach a destination outside its own subnet, it cannot send the packet directly.

Instead, it sends the packet to its gateway.

The gateway must be the router interface connected to the host network.

---

### Solving Strategy

For the host:

```text
0.0.0.0/0 => nearest router interface
```

The right side must be reachable from the host.

That means the gateway IP must be in the same subnet as the host interface.

---

### Why It Works

The host sends unknown traffic to the router.

The router then forwards the packet toward the correct destination.

---

### Common Mistake

Wrong:

```text
0.0.0.0/0 => destination host IP
```

Correct:

```text
0.0.0.0/0 => nearest router interface
```

The gateway is the next hop, not the final destination.

---

### Key Takeaway

A gateway must be directly reachable.

---

### Configuration File

[`configs/level3.json`](configs/level3.json)

</details>

---

<details>
<summary><strong>Level 4 - Routing Table Basics</strong></summary>

<br>

<img src="screenshots/level4.png" alt="Level 4" width="900"/>

### Goal

This level focuses on routing tables.

A router must know how to forward traffic to a network that is not directly connected.

---

### Network Analysis

A router automatically knows the networks connected to its own interfaces.

However, it does not automatically know remote networks.

For remote networks, we must add routes manually.

---

### Solving Strategy

For each router:

1. Identify directly connected networks.
2. Identify remote networks.
3. Add routes only for networks that are not directly connected.
4. Make sure the gateway is a directly reachable next hop.

---

### Example

```text
Remote network => next router interface
```

Example:

```text
192.168.2.0/24 => 192.168.1.2
```

---

### Why It Works

The router checks the destination IP against its routing table.

If a matching route exists, the router forwards the packet to the next hop.

---

### Key Takeaway

Routers need routes to reach networks that are not directly connected.

---

### Configuration File

[`configs/level4.json`](configs/level4.json)

</details>

---

<details>
<summary><strong>Level 5 - Multiple Networks</strong></summary>

<br>

<img src="screenshots/level5.png" alt="Level 5" width="900"/>

### Goal

This level contains multiple networks connected through routers.

The goal is to correctly separate the topology into different subnets and configure routes between them.

---

### Network Analysis

The most important step is to split the diagram into smaller networks.

A common mistake is treating the whole diagram as one big network.

That is wrong.

Each router interface usually belongs to a different subnet.

---

### Solving Strategy

I solve this type of level by writing the topology as small groups:

```text
Network 1: Host + Router interface
Network 2: Router interface + Router interface
Network 3: Router interface + Host
```

Then I configure each group separately.

---

### Why It Works

Once each link has a valid subnet, routing becomes easier.

Each host sends traffic to its nearest router.

Each router forwards traffic based on its routing table.

---

### Key Takeaway

Do not look at the full topology all at once.

Split it into small networks first.

---

### Configuration File

[`configs/level5.json`](configs/level5.json)

</details>

---

<details>
<summary><strong>Level 6 - Internet and Reverse Routes</strong></summary>

<br>

<img src="screenshots/level6.png" alt="Level 6" width="900"/>

### Goal

This level introduces communication with the Internet.

The local host must reach the Internet, and the Internet must be able to send traffic back.

---

### Network Analysis

The forward path is not enough.

If a host can reach the Internet, the Internet must also have a route back to the host network.

This is why reverse routes are important.

---

### Solving Strategy

For the local network:

```text
Host => Router => Internet
```

For the reverse path:

```text
Internet => Router => Host
```

The Internet route should point back to the internal network through the router interface connected to the Internet.

---

### Common Mistake

Wrong:

```text
Internet route:
163.172.250.12 => 163.172.250.12
```

Correct idea:

```text
Internal network => router interface connected to Internet
```

Example:

```text
192.168.1.0/24 => 163.172.250.12
```

---

### Key Takeaway

When the goal includes the Internet, always check the reverse route.

---

### Configuration File

[`configs/level6.json`](configs/level6.json)

</details>

---

<details>
<summary><strong>Level 7 - Router-to-Router Routing</strong></summary>

<br>

<img src="screenshots/level7.png" alt="Level 7" width="900"/>

### Goal

This level requires communication across multiple routers.

The goal is to make hosts communicate through a chain of routers.

---

### Network Analysis

A router-to-router link is usually a small point-to-point network.

For two router interfaces, a `/30` subnet is usually enough.

Example:

```text
Router 1 <--> Router 2
```

Possible subnet:

```text
10.0.0.0/30
```

Usable IPs:

```text
10.0.0.1
10.0.0.2
```

---

### Solving Strategy

1. Assign a valid subnet to each link.
2. Make sure router-to-router interfaces are in the same subnet.
3. Add routes on each router for remote networks.
4. Check both forward and reverse paths.

---

### Why It Works

Each router forwards the packet one step closer to the destination.

The return traffic must also know how to go back.

---

### Key Takeaway

For router-to-router links, `/30` is usually the cleanest choice.

---

### Configuration File

[`configs/level7.json`](configs/level7.json)

</details>

---

<details>
<summary><strong>Level 8 - Forward and Reverse Path Troubleshooting</strong></summary>

<br>

<img src="screenshots/level8.png" alt="Level 8" width="900"/>

### Goal

This level focuses heavily on troubleshooting routes.

Some packets may reach the destination, but the response may fail.

---

### Network Analysis

If the status says:

```text
No reverse way
```

that means the forward path works, but the return path is missing or wrong.

If the status says:

```text
Loop detected
```

that means packets are being sent back and forth between devices.

---

### Solving Strategy

I use the logs to find the first real error.

Important log messages:

```text
destination does not match any route
invalid route
route match but no interface for gateway
loop detected
```

These messages usually point directly to the wrong route or gateway.

---

### Why It Works

By fixing the first real error in the log, the network becomes easier to debug.

Fixing random values usually creates more problems.

---

### Key Takeaway

Read the logs carefully.

They usually tell you exactly where the routing problem starts.

---

### Configuration File

[`configs/level8.json`](configs/level8.json)

</details>

---

<details>
<summary><strong>Level 9 - Complex Routing</strong></summary>

<br>

<img src="screenshots/level9.png" alt="Level 9" width="900"/>

### Goal

This level combines multiple hosts, switches, routers, and Internet routes.

The goal is to make several networks communicate correctly.

---

### Network Analysis

This level should be solved by grouping the topology into separate networks.

Example structure:

```text
Network 1: hosts connected to a switch
Network 2: router-to-router link
Network 3: host network behind router
Network 4: Internet link
```

A switch keeps devices in the same network.

A router separates networks.

---

### Solving Strategy

For every network:

1. Count the required usable IP addresses.
2. Choose the smallest valid subnet.
3. Assign valid usable IP addresses.
4. Avoid network and broadcast addresses.
5. Add routes between routers.
6. Add Internet reverse routes.

---

### Why It Works

The solution works when every device knows where to send the packet and where to send the reply.

For complex levels, reverse routes become just as important as forward routes.

---

### Key Takeaway

In large topologies, the first step is always subnet separation.

Do not start with routes before identifying the networks.

---

### Configuration File

[`configs/level9.json`](configs/level9.json)

</details>

---

<details>
<summary><strong>Level 10 - Full Network Troubleshooting</strong></summary>

<br>

<img src="screenshots/level10.png" alt="Level 10" width="900"/>

### Goal

This is the final NetPractice level.

It combines all the important concepts:

* Direct host communication
* Switch-based networks
* Router-to-router links
* Multiple subnets
* Default gateways
* Routing tables
* Internet reverse routes
* Troubleshooting logs

---

### Network Analysis

The topology contains several internal networks and an Internet connection.

To solve it correctly, each part must be separated:

```text
Hosts on the same switch => same subnet
Router-to-router link => separate subnet
Hosts behind a router => separate subnet
Internet link => separate subnet
```

---

### Solving Strategy

I solved this level by following this order:

1. Fix directly connected hosts first.
2. Fix switch networks.
3. Fix router-to-router links.
4. Configure host gateways.
5. Configure router routes.
6. Configure Internet reverse routes.
7. Read logs and fix one error at a time.

---

### Why It Works

The final configuration works because every network has:

* Valid IP addresses
* Correct subnet masks
* Reachable gateways
* Correct forward routes
* Correct reverse routes

No packet is sent to an unreachable gateway, and no route sends traffic in a loop.

---

### Key Takeaway

The final level is not solved by guessing.

It is solved by breaking the topology into small networks and validating each path step by step.

---

### Configuration File

[`configs/level10.json`](configs/level10.json)

</details>

---

## Common NetPractice Errors

| Error                                      | Meaning                                                 | How to Fix                                                |
| ------------------------------------------ | ------------------------------------------------------- | --------------------------------------------------------- |
| `invalid IP address`                       | The IP is probably a network or broadcast address       | Recalculate the subnet and choose a usable IP             |
| `no route`                                 | A route is missing                                      | Add a route to the destination network                    |
| `invalid route`                            | The gateway is not reachable                            | Make sure the gateway is in the same subnet as the sender |
| `loop detected`                            | Traffic is being sent back and forth                    | Check default routes and router routes                    |
| `wrong host`                               | The packet reached the right IP but on the wrong device | Check for duplicate IPs or overlapping subnets            |
| `no reverse way`                           | Forward path works but return path is missing           | Add a reverse route                                       |
| `route match but no interface for gateway` | The route points to a gateway the device cannot reach   | Replace the gateway with a directly connected next hop    |

---

## Important Rules

* A switch does not separate networks.
* A router separates networks.
* Every router interface usually belongs to a different subnet.
* The gateway must be directly reachable.
* The right side of a route must be a gateway IP.
* The left side of a route must be a destination network.
* `0.0.0.0/0` means any destination.
* Network addresses cannot be assigned to devices.
* Broadcast addresses cannot be assigned to devices.
* Communication needs both a forward path and a reverse path.

---

## Best Practices

* Use `/30` for point-to-point links.
* Use the smallest subnet that fits the number of devices.
* Do not waste IP addresses when a smaller subnet is enough.
* Do not use a default route when a specific route is required.
* Do not create multiple default routes unless you fully understand the effect.
* Always check the first real error in the log.
* Do not fix random values without understanding the topology.

---

## 42 Note

This repository is intended for learning, review, and documentation.

If you are a 42 student working on NetPractice, try to solve each level by yourself first.

Do not copy the configurations blindly.

The real goal of NetPractice is to understand how IP addressing, subnetting, routing, and gateways work.

---

## Status

Completed.

---

## Author

Mohammad Alhindi

* GitHub: [@mohammadalhindi1](https://github.com/mohammadalhindi1)
