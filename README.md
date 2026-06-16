# NetPractice

NetPractice is a 42 networking project focused on IP addressing, subnet masks, routing tables, gateways, and troubleshooting network communication.

This repository contains my solved configurations, screenshots, and explanations for all 10 levels.

## Contents

- [Basics](#basics)
- [Subnet Planning](#subnet-planning)
- [Levels](#levels)
- [Common Errors](#common-errors)

## Basics

### IP Address

An IP address identifies an interface inside a network.

### Subnet Mask

A subnet mask defines the network size and determines which IP addresses belong to the same network.

### Gateway

A gateway is the next hop used to reach another network.

### Routing Table

A routing table tells a host or router where to send packets based on the destination network.

## Subnet Planning

| Required usable IPs | Best subnet | Mask |
|---:|---:|---:|
| 2 | `/30` | `255.255.255.252` |
| 3 - 6 | `/29` | `255.255.255.248` |
| 7 - 14 | `/28` | `255.255.255.240` |
| 15 - 30 | `/27` | `255.255.255.224` |
| 31 - 62 | `/26` | `255.255.255.192` |
| 63 - 126 | `/25` | `255.255.255.128` |
| 127 - 254 | `/24` | `255.255.255.0` |

## Levels

<details>
<summary>Level 1 - Direct Host Communication</summary>

![Level 1](screenshots/level1.png)

### Goal

Two direct host-to-host communications:

- Host A to Host B
- Host C to Host D

### Explanation

There are two separate networks.  
Each pair of connected hosts must belong to the same subnet.

No router is used in this level, so no gateway is required.

### Key Rule

Devices connected directly must share the same subnet.

</details>

<details>
<summary>Level 2</summary>

![Level 2](screenshots/level2.png)

Explanation will be added here.

</details>

<details>
<summary>Level 3</summary>

![Level 3](screenshots/level3.png)

Explanation will be added here.

</details>

<details>
<summary>Level 4</summary>

![Level 4](screenshots/level4.png)

Explanation will be added here.

</details>

<details>
<summary>Level 5</summary>

![Level 5](screenshots/level5.png)

Explanation will be added here.

</details>

<details>
<summary>Level 6</summary>

![Level 6](screenshots/level6.png)

Explanation will be added here.

</details>

<details>
<summary>Level 7</summary>

![Level 7](screenshots/level7.png)

Explanation will be added here.

</details>

<details>
<summary>Level 8</summary>

![Level 8](screenshots/level8.png)

Explanation will be added here.

</details>

<details>
<summary>Level 9</summary>

![Level 9](screenshots/level9.png)

Explanation will be added here.

</details>

<details>
<summary>Level 10</summary>

![Level 10](screenshots/level10.png)

Explanation will be added here.

</details>

## Common Errors

| Error | Meaning |
|---|---|
| `invalid IP address` | The IP is probably a network or broadcast address |
| `no route` | A route is missing |
| `invalid route` | The gateway is not reachable |
| `loop detected` | Traffic is being sent back and forth between routers |
| `wrong host` | The packet reached the right IP but on the wrong device |
| `no reverse way` | Forward path works, but the return path is missing |

## 42 Note

This repository is for learning and revision.  
If you are working on NetPractice, try to solve each level first before comparing with these notes.

## Author

Mohammad Alhindi
