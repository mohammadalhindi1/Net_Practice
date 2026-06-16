# NetPractice

<p align="center">
  <img src="img/level8.png" alt="NetPractice Final Level Preview" width="900"/>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/42-NetPractice-blue" alt="42 NetPractice"/>
  <img src="https://img.shields.io/badge/Networking-IPv4-green" alt="Networking"/>
  <img src="https://img.shields.io/badge/Status-Completed-brightgreen" alt="Completed"/>
</p>

## About

This repository contains my solutions and explanations for the **NetPractice** project from the **42 curriculum**.

NetPractice is a networking project focused on configuring IPv4 addresses, subnet masks, gateways, and routing tables to make different hosts communicate correctly.

The purpose of this repository is not just to store the final answers, but to document the way I think through each level, how I split the network, how I choose valid subnets, and how I debug routing errors.

This guide is written as a study reference for myself and for anyone learning the basics of networking through NetPractice.

---

## Contents

* [Repository Structure](#repository-structure)
* [Basics](#basics)

  * [IPv4 Addresses](#ipv4-addresses)
  * [Special IP Ranges](#special-ip-ranges)
  * [Masks](#masks)
  * [Switches](#switches)
  * [Routers](#routers)
  * [Routing Tables](#routing-tables)
  * [Network Logic](#network-logic)
* [How I Approach NetPractice](#how-i-approach-netpractice)
* [Common Errors](#common-errors)
* [Levels](#levels)

  * [Level 1](#level-1)
  * [Level 2](#level-2)
  * [Level 3](#level-3)
  * [Level 4](#level-4)
  * [Level 5](#level-5)
  * [Level 6](#level-6)
  * [Level 7](#level-7)
  * [Level 8](#level-8)
  * [Level 9](#level-9)
  * [Level 10](#level-10)
* [42 Note](#42-note)
* [Author](#author)

---

## Repository Structure

```text
.
├── README.md
├── img/
│   ├── level1.png
│   ├── level2.png
│   ├── level3.png
│   ├── level4.png
│   ├── level5.png
│   ├── level6.png
│   ├── level7.png
│   ├── level8.png
│   ├── level9.png
│   └── level10.png
└── solution/
    ├── level1.json
    ├── level2.json
    ├── level3.json
    ├── level4.json
    ├── level5.json
    ├── level6.json
    ├── level7.json
    ├── level8.json
    ├── level9.json
    └── level10.json
```

| Folder      | Description                                                |
| ----------- | ---------------------------------------------------------- |
| `img/`      | Contains screenshots for every solved level                |
| `solution/` | Contains the exported NetPractice JSON configuration files |
| `README.md` | Contains the explanation, concepts, and level walkthroughs |

---

## Basics

For this project, we only deal with **IPv4**.

An IPv4 address is a 32-bit number divided into four blocks. Each block is 8 bits.

Example:

```text
192.168.100.1
```

In binary:

```text
11000000.10101000.01100100.00000001
```

Each block can have a value from:

```text
0 to 255
```

The same binary logic applies to subnet masks.

Example:

```text
255.255.255.0
```

In binary:

```text
11111111.11111111.11111111.00000000
```

After a bit becomes `0` in a subnet mask, there cannot be any `1` bits after it.

That is why these values are valid inside masks:

```text
255 = 11111111
254 = 11111110
252 = 11111100
248 = 11111000
240 = 11110000
224 = 11100000
192 = 11000000
128 = 10000000
0   = 00000000
```

So this is a valid mask:

```text
255.255.255.0
```

But this is not a valid mask:

```text
255.255.128.128
```

because after the mask starts using `0` bits, it cannot go back to `1` bits.

---

## IPv4 Addresses

An IP address identifies an interface inside a network.

In NetPractice, we do not configure devices directly. We configure their interfaces.

Example:

```text
interface A1
IP:   192.168.1.2
Mask: 255.255.255.0
```

This means interface `A1` belongs to the network:

```text
192.168.1.0/24
```

Two devices can communicate directly only if they are in the same network, or if there is a router between their networks.

---

## Special IP Ranges

Some IP ranges are reserved for private networks.

These private ranges are:

```text
10.0.0.0      - 10.255.255.255
172.16.0.0    - 172.31.255.255
192.168.0.0   - 192.168.255.255
```

The loopback range is:

```text
127.0.0.0 - 127.255.255.255
```

For NetPractice, the most important thing is not whether an IP is public or private. The most important thing is whether the IP belongs to the correct subnet.

---

## Masks

A subnet mask decides which IP addresses are part of the same network.

There are two common ways to write a subnet mask:

```text
Dot-decimal notation: 255.255.255.0
CIDR notation:        /24
```

Example:

```text
192.168.1.10/24
```

means:

```text
IP:   192.168.1.10
Mask: 255.255.255.0
```

The first address in a subnet is the **network address**.

The last address in a subnet is the **broadcast address**.

Both cannot be assigned to devices.

---

### Subnet Cheat Sheet

|  CIDR |  Dot-decimal Mask | Total IPs | Usable IPs |
| ----: | ----------------: | --------: | ---------: |
| `/32` | `255.255.255.255` |         1 |          0 |
| `/31` | `255.255.255.254` |         2 |          0 |
| `/30` | `255.255.255.252` |         4 |          2 |
| `/29` | `255.255.255.248` |         8 |          6 |
| `/28` | `255.255.255.240` |        16 |         14 |
| `/27` | `255.255.255.224` |        32 |         30 |
| `/26` | `255.255.255.192` |        64 |         62 |
| `/25` | `255.255.255.128` |       128 |        126 |
| `/24` |   `255.255.255.0` |       256 |        254 |
| `/16` |     `255.255.0.0` |     65536 |      65534 |

Example with `/30`:

```text
Network:   190.3.2.252
Usable:    190.3.2.253
Usable:    190.3.2.254
Broadcast: 190.3.2.255
```

Only the usable IP addresses can be assigned to interfaces.

---

## Switches

A switch connects multiple devices inside the same network.

A switch does not separate networks.

If several devices are connected to the same switch, they usually need to be in the same subnet.

Example:

```text
Host A
Host B
Router interface
```

If all of them are connected to the same switch, they should share the same network.

---

## Routers

A router connects different networks.

A router can have multiple interfaces, and each interface usually belongs to a different subnet.

Example:

```text
R1 interface 1 -> Network A
R1 interface 2 -> Network B
```

The router can forward packets between these networks only if the routes are correct.

A router does not magically know every network. It only knows:

1. Networks directly connected to its own interfaces.
2. Networks manually added in its routing table.

---

## Routing Tables

A routing table tells a host or router where to send packets.

In NetPractice, a route has two parts:

```text
Destination => Next hop
```

Example:

```text
192.168.2.0/24 => 192.168.1.1
```

Meaning:

```text
To reach 192.168.2.0/24, send the packet to 192.168.1.1.
```

The left side is the destination network.

The right side is the next hop or gateway.

Important rule:

```text
The gateway must be directly reachable.
```

That means the gateway must be in the same subnet as one of the sender's interfaces.

---

### Default Route

A default route is used when no more specific route matches.

It can be written as:

```text
default
```

or:

```text
0.0.0.0/0
```

Both mean:

```text
Any destination.
```

Example:

```text
0.0.0.0/0 => 192.168.1.1
```

This means:

```text
Send all unknown traffic to 192.168.1.1.
```

---

## Network Logic

To know whether two devices are part of the same network, combine the IP address with the subnet mask.

This is done using a bit-by-bit AND operation.

Example:

```text
IP:   192.168.100.1
Mask: 255.255.255.0
```

Binary:

```text
IP:   11000000.10101000.01100100.00000001
Mask: 11111111.11111111.11111111.00000000
```

Result:

```text
11000000.10101000.01100100.00000000
```

In dot-decimal:

```text
192.168.100.0
```

So the network is:

```text
192.168.100.0/24
```

If two devices have the same network address, they are in the same network.

If they are not in the same network, they need a router to communicate.

---

## How I Approach NetPractice

My solving process:

1. Read the goal.
2. Identify which hosts need to communicate.
3. Split the topology into separate networks.
4. Check all directly connected interfaces.
5. Make sure devices on the same link are in the same subnet.
6. Avoid network and broadcast addresses.
7. Set host gateways to the nearest router.
8. Add routes on routers for remote networks.
9. Add reverse routes when the Internet is involved.
10. Read the logs and fix the first real error.

The most important rule:

```text
Do not solve the whole diagram at once.
Split it into small networks first.
```

---

## Common Errors

| Error                                      | Meaning                                       | Fix                                        |
| ------------------------------------------ | --------------------------------------------- | ------------------------------------------ |
| `invalid IP address`                       | The IP is a network or broadcast address      | Choose a usable IP                         |
| `no route`                                 | A route is missing                            | Add a route to the destination network     |
| `invalid route`                            | The gateway is not reachable                  | Use a gateway in the same subnet           |
| `loop detected`                            | Traffic is being sent back and forth          | Fix wrong default routes                   |
| `wrong host`                               | The IP exists on the wrong device             | Check duplicate IPs or overlapping subnets |
| `no reverse way`                           | Forward path works but return path is missing | Add the return route                       |
| `route match but no interface for gateway` | The gateway is not directly reachable         | Use the correct next hop                   |

---

## Levels

Here are the solutions and explanations for all 10 levels.

---

## Level 1

<details>
<summary><strong>show</strong></summary>

<br>

<img src="img/level1.png" alt="Level 1" width="900"/>

### Goal

This level has two separate communication goals:

```text
A <--> B
C <--> D
```

There are no routers.

---

### Explanation

Each pair of hosts is directly connected.

That means:

```text
A1 and B1 must be in the same subnet.
C1 and D1 must be in the same subnet.
```

No gateway is needed because there is no router.

---

### Why It Works

For the first connection:

```text
A1 = 104.95.23.11
B1 = 104.95.23.12
Mask = 255.255.255.0
```

Both are inside:

```text
104.95.23.0/24
```

For the second connection:

```text
C1 = 211.191.135.75
D1 = 211.191.135.74
Mask = 255.255.0.0
```

Both are inside:

```text
211.191.0.0/16
```

So both pairs can communicate directly.

---

### Key Takeaway

Directly connected devices must be part of the same subnet.

---

### Solution File

[`solution/level1.json`](solution/level1.json)

</details>

[back to contents](#contents)

---

## Level 2

<details>
<summary><strong>show</strong></summary>

<br>

<img src="img/level2.png" alt="Level 2" width="900"/>

### Goal

This level focuses on making directly connected devices communicate by fixing IP addresses and subnet masks.

---

### Explanation

When two interfaces are directly connected, they must belong to the same subnet.

A gateway is not needed unless the destination is outside the local network.

---

### How to Think

For each connected pair:

1. Read the IP address.
2. Read the mask.
3. Calculate the network.
4. Make sure both interfaces belong to the same network.
5. Make sure neither IP is the network address or broadcast address.

---

### Key Takeaway

The IP address alone is not enough.

You must always check:

```text
IP + Mask
```

---

### Solution File

[`solution/level2.json`](solution/level2.json)

</details>

[back to contents](#contents)

---

## Level 3

## Level 3

<details>
<summary><strong>show</strong></summary>

<br>

<img src="img/level3.png" alt="Level 3" width="900"/>

### Goal

This level has three communication goals:

```text
A <--> B
A <--> C
B <--> C
```

All three hosts are connected to the same switch.

---

### Network Topology

The topology can be simplified like this:

```text
        Switch S
       /   |   \
      A    B    C
```

There is no router in this level.

That means all connected hosts must be part of the same local network.

---

### Network Analysis

The switch connects all hosts inside the same network.

A switch does not separate networks.
It only forwards traffic between devices connected to it.

So the important rule here is:

```text
A, B, and C must be in the same subnet.
```

From the successful log, the hosts communicate using these IP addresses:

```text
A = 104.198.223.125
B = 104.198.223.123
C = 104.198.223.124
```

All of them are inside the same IP range:

```text
104.198.223.x
```

So they can communicate directly through the switch.

---

### Why No Gateway Is Needed

There is no router in this level.

Since all hosts are in the same subnet, packets do not need to leave the local network.

So no default gateway is required.

The hosts can send packets directly to each other through the switch.

---

### Understanding the Log

Example from Goal 1:

```text
Forward way: A -> B
on A: packet accepted
on A: send to A1
on switch S: pass to all connections
on B: packet accepted
on B: destination IP reached
```

This means:

1. Host A accepted the packet.
2. A sent the packet through interface `A1`.
3. The switch forwarded the packet to connected devices.
4. Host B received the packet.
5. The destination was reached successfully.

---

### Why `packet not for me` Appears

In the log, we also see:

```text
on C: packet not for me
```

This is normal.

Because the switch forwards traffic to connected devices, another host may see the packet but ignore it if the destination IP is not its own IP.

So when A sends a packet to B, host C may receive the packet but rejects it because it is not the destination.

---

### Why `loop detected` Appears

The log also shows:

```text
on A: loop detected
```

or:

```text
on B: loop detected
```

In this level, this is not the main problem because the final result is successful.

The important line is:

```text
destination IP reached
```

The switch tests all connected links, including the sender itself.
When the packet is seen again by the original sender, NetPractice marks it as a loop in the log.

As long as the destination is reached and the status is `OK`, this is not an issue for this level.

---

### Final Result

All communication goals work:

```text
A -> B : OK
A -> C : OK
B -> C : OK
```

And the reverse paths also work:

```text
B -> A : OK
C -> A : OK
C -> B : OK
```

---

### Key Takeaway

When multiple hosts are connected to the same switch:

```text
They must belong to the same subnet.
```

A switch does not route traffic between networks.

If the hosts are in different subnets, a router would be needed.

---

### Best Practice

For a switch network, count how many usable IP addresses are needed.

In this level, there are three hosts:

```text
A
B
C
```

So the subnet must provide at least three usable IP addresses.

A `/30` would not be enough because it gives only two usable IP addresses.

A `/29` or larger subnet would work because `/29` gives six usable IP addresses.

---

### Solution File

[`solution/level3.json`](solution/level3.json)

</details>

[back to contents](#contents)

---

## Level 4

<details>
<summary><strong>show</strong></summary>

<br>

<img src="img/level4.png" alt="Level 4" width="900"/>

### Goal

This level focuses on router routing tables.

The router must know where to forward traffic for remote networks.

---

### Explanation

A router automatically knows only the networks directly connected to its own interfaces.

For any remote network, a route must be added manually.

---

### How to Think

For each router:

```text
What networks am I directly connected to?
What networks are remote?
Which next hop leads to those remote networks?
```

---

### Key Takeaway

Routers need routes for networks that are not directly connected.

---

### Solution File

[`solution/level4.json`](solution/level4.json)

</details>

[back to contents](#contents)

---

## Level 5

<details>
<summary><strong>show</strong></summary>

<br>

<img src="img/level5.png" alt="Level 5" width="900"/>

### Goal

This level contains multiple networks.

The goal is to correctly split the topology into separate subnets and connect them using routers.

---

### Explanation

The biggest mistake in this type of level is treating the whole diagram as one network.

That is wrong.

A router separates networks.

Each router interface usually belongs to a different subnet.

---

### How to Think

Split the topology like this:

```text
Network 1: Host + Router interface
Network 2: Router interface + Router interface
Network 3: Router interface + Host
```

Solve each small network first, then configure routes.

---

### Key Takeaway

Do not start with routes.

First, identify the networks.

---

### Solution File

[`solution/level5.json`](solution/level5.json)

</details>

[back to contents](#contents)

---

## Level 6

<details>
<summary><strong>show</strong></summary>

<br>

<img src="img/level6.png" alt="Level 6" width="900"/>

### Goal

This level introduces Internet communication.

The host must reach the Internet, and the Internet must know how to send traffic back.

---

### Explanation

Forward traffic is not enough.

If a packet goes from the host to the Internet, the reply must know how to return.

That is why the Internet box often needs a route back to the internal network.

---

### Correct Internet Route Idea

```text
Internal network => router interface connected to Internet
```

Example:

```text
192.168.1.0/24 => 163.172.250.12
```

---

### Key Takeaway

When the Internet is involved, always check the reverse path.

---

### Solution File

[`solution/level6.json`](solution/level6.json)

</details>

[back to contents](#contents)

---

## Level 7

<details>
<summary><strong>show</strong></summary>

<br>

<img src="img/level7.png" alt="Level 7" width="900"/>

### Goal

This level uses multiple routers.

The goal is to make hosts communicate through a router-to-router path.

---

### Explanation

A link between two routers is usually a point-to-point network.

For two devices, a `/30` subnet is usually enough.

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

### How to Think

1. Make sure router-to-router interfaces are in the same subnet.
2. Make sure each host has the correct gateway.
3. Add routes for remote networks.
4. Check the reverse path.

---

### Key Takeaway

For router-to-router links, `/30` is usually the cleanest subnet.

---

### Solution File

[`solution/level7.json`](solution/level7.json)

</details>

[back to contents](#contents)

---

## Level 8

<details>
<summary><strong>show</strong></summary>

<br>

<img src="img/level8.png" alt="Level 8" width="900"/>

### Goal

This level focuses on troubleshooting forward and reverse routes.

---

### Explanation

If the forward path works but the reverse path does not, NetPractice may show:

```text
No reverse way
```

If traffic keeps moving between the same devices, it may show:

```text
Loop detected
```

---

### How to Debug

Read the log and find the first real routing error.

Important messages:

```text
destination does not match any route
invalid route
route match but no interface for gateway
loop detected
```

These messages usually point to the wrong route or gateway.

---

### Key Takeaway

Do not randomly change IPs.

Use the log to find where the packet fails.

---

### Solution File

[`solution/level8.json`](solution/level8.json)

</details>

[back to contents](#contents)

---

## Level 9

<details>
<summary><strong>show</strong></summary>

<br>

<img src="img/level9.png" alt="Level 9" width="900"/>

### Goal

This level combines several concepts:

```text
Hosts
Switches
Routers
Multiple subnets
Internet routes
Reverse paths
```

---

### Explanation

For larger levels, the correct approach is to split the diagram into separate networks.

A switch keeps devices in the same network.

A router separates networks.

---

### How to Think

For every section:

1. Count the required usable IP addresses.
2. Choose a subnet that fits.
3. Assign valid IPs.
4. Avoid network and broadcast addresses.
5. Configure host gateways.
6. Configure router routes.
7. Configure Internet reverse routes.

---

### Key Takeaway

In complex topologies, subnet separation is the first step.

Routes come after that.

---

### Solution File

[`solution/level9.json`](solution/level9.json)

</details>

[back to contents](#contents)

---

## Level 10

<details>
<summary><strong>show</strong></summary>

<br>

<img src="img/level10.png" alt="Level 10" width="900"/>

### Goal

This is the final NetPractice level.

It combines everything:

```text
Direct communication
Switch networks
Router-to-router links
Subnetting
Default gateways
Routing tables
Internet reverse routes
Log debugging
```

---

### Explanation

The final level should be solved step by step.

Do not try to solve the whole diagram at once.

Start with the directly connected hosts, then move to switches, routers, and finally the Internet routes.

---

### Solving Order

```text
1. Direct host communication
2. Switch networks
3. Router-to-router links
4. Host gateways
5. Router routes
6. Internet reverse routes
7. Logs and final debugging
```

---

### Key Takeaway

Level 10 is not hard because of one complex idea.

It is hard because it combines many simple ideas at the same time.

---

### Solution File

[`solution/level10.json`](solution/level10.json)

</details>

[back to contents](#contents)

---

## 42 Note

This repository is for learning, review, and documentation.

If you are a 42 student working on NetPractice, try to solve each level by yourself first.

Do not copy the configurations blindly.

The real goal of NetPractice is to understand how IP addressing, subnetting, gateways, and routing tables work.

---

## Status

Completed.

---

## Author

Mohammad Alhindi

* GitHub: [@mohammadalhindi1](https://github.com/mohammadalhindi1)
