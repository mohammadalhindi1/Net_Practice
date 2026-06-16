# NetPractice - 42 Network Configuration Project

This repository contains my solutions and notes for the **NetPractice** project from the **42 curriculum**.

NetPractice is a practical networking exercise where the goal is to configure IP addresses, subnet masks, gateways, and routing tables so that different hosts can communicate correctly across multiple networks.

## Project Overview

The project focuses on understanding the basics of computer networking through visual network scenarios.

The main concepts covered are:

* IP addressing
* Subnet masks
* Network addresses
* Broadcast addresses
* Usable host ranges
* Default gateways
* Routing tables
* Router-to-router communication
* Host-to-host communication
* Internet reverse routes
* Troubleshooting routing errors

## Repository Content

This repository includes JSON configuration files for each level:

| Level    | File           |
| -------- | -------------- |
| Level 1  | `level1.json`  |
| Level 2  | `level2.json`  |
| Level 3  | `level3.json`  |
| Level 4  | `level4.json`  |
| Level 5  | `level5.json`  |
| Level 6  | `level6.json`  |
| Level 7  | `level7.json`  |
| Level 8  | `level8.json`  |
| Level 9  | `level9.json`  |
| Level 10 | `level10.json` |

Each file represents the saved configuration for one NetPractice level.

## What I Learned

While working on this project, I learned how to analyze a network step by step instead of guessing values.

The most important rules I used were:

### 1. Devices on the same link must be in the same subnet

If two interfaces are directly connected, they must share the same network.

Example:

```text
Host A: 192.168.1.2/24
Router: 192.168.1.1/24
```

Both belong to:

```text
192.168.1.0/24
```

### 2. A gateway must be directly reachable

A gateway is the next hop, not the final destination.

A host should usually send unknown traffic to the router interface connected to its own network.

Example:

```text
0.0.0.0/0 => 192.168.1.1
```

### 3. Routers need routes to remote networks

A router only knows its directly connected networks.
For any remote network, a route must be added manually.

Example:

```text
10.0.2.0/24 => 10.0.1.2
```

### 4. Communication needs a forward path and a reverse path

If host A can reach host B, host B must also be able to send traffic back to host A.

Many NetPractice errors happen because the forward route works but the reverse route is missing.

### 5. Network and broadcast addresses cannot be used by hosts

For example, in:

```text
192.168.1.0/30
```

The addresses are:

```text
192.168.1.0   Network address
192.168.1.1   Usable
192.168.1.2   Usable
192.168.1.3   Broadcast address
```

Only the usable addresses can be assigned to interfaces.

## Common Errors I Faced

| Error                | Meaning                                                   |
| -------------------- | --------------------------------------------------------- |
| `invalid IP address` | The IP is probably a network or broadcast address         |
| `no route`           | A route is missing                                        |
| `invalid route`      | The gateway is not reachable                              |
| `loop detected`      | Traffic is being sent in a loop between devices           |
| `wrong host`         | The packet reached an IP that belongs to the wrong device |
| `no reverse way`     | Forward path works, but the return path is missing        |

## How I Approach Each Level

My solving process:

1. Identify which hosts need to communicate.
2. Split the diagram into separate networks.
3. Check which devices are connected by switches or direct links.
4. Assign valid IP addresses and subnet masks.
5. Configure host default gateways.
6. Add router routes for remote networks.
7. Check the reverse path.
8. Read the logs and fix the first real error.

## Important Notes

* A switch does not separate networks.
* A router separates networks.
* Every router interface usually belongs to a different subnet.
* `0.0.0.0/0` means "any destination".
* The right side of a route must be a reachable gateway IP, not a network address.
* The left side of a route is the destination network.

## 42 Project Rules

This repository is intended for learning and revision.

If you are a 42 student, do not copy the solutions blindly.
Use them to compare your reasoning after trying to solve the levels yourself.

The goal of NetPractice is not only to pass the project, but to understand how IP addressing and routing work.

## Status

Project completed.

## Author

Mohammad Alhindi

* GitHub: [@mohammadalhindi1](https://github.com/mohammadalhindi1)
