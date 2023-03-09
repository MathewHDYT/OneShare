---
layout: default
title: LAN
parent: CyberSecurity
grand_parent: Notes
---

## LAN

### Meaning:

Means `Local Area Network` (`LAN`).
Over the years, there has been experimentation and implementation of various **network** designs.
One of that aspect is the **topology**, which refers to the design or look of the **network**.
These include but are not limited to:

### Star Topology:

The main premise is that devices are individually connected via a central networking device such as a **switch** or **hub**.
It is the most commonly found today because of its reliability and scalability, despite its higher cost.

Therefore, any information sent to a device in this topology is sent via the central device to which it connects.

![Star](https://raw.githubusercontent.com/MathewHDYT/OneShare/main/_images/star.png)

**ADVANTAGES** | **DISADVANTAGE** |
-------------- | ---------------- |
Higher scalability (easy to add new devices if the need arises). Reduced rate of failure | More expensive. Scaling the network, also increases the maintenance required to keep it functional --> makes troubleshooting faults much harder. Can still fail if the centralized hardware that connects devices fails |

### Bus Topology: 

It relies upon a single connection, which is known as a backbone cable.
This type of topology is similar to the leaf of a tree in the sense that **devices** (leaves) stem from where the branches are on this cable.

![Bus](https://raw.githubusercontent.com/MathewHDYT/OneShare/main/_images/bus.png)

**ADVANTAGES** | **DISADVANTAGE** |
-------------- | ---------------- |
Easy and cost-efficient to set up, because of not needed cabling or dedicated networking equipment for connecting the devices | Very quickly prone to become slow and bottle necked if devices withing the topology are simultaneously requesting data. Difficulty to troubleshoot, because it becomes hard to identify which device is experiencing issues with data all traveling along the same route. Little redundancy in case of failure, because of a single point of failure along the backbone cable (if it breaks, all devices can no longer receive or transmit data along the bus) |

### Ring Topology:

Also known as **token topology**, boasts some similarities to the **bus topology**.
**Devices** such as computers are directly connected to each other to form a loop meaning that there is little cabling required and less dependence on dedicated hardware.

![Ring](https://raw.githubusercontent.com/MathewHDYT/OneShare/main/_images/ring.png)

It works by sending data across the loop until it reaches the destined **device**, using other devices along the loop to forward the data. Interestingly, a **device** will only send data from another **device** in this topology if it does not have any to send itself. If the **device** happens to have data to send, it will send its own data first before sending data from another **device**. 

**ADVANTAGES** | **DISADVANTAGE** |
-------------- | ---------------- |
Easy and cost-efficient to set up, because of not needed cabling or dedicated networking equipment for connecting the devices. Because there is only one direction for data to travel across, it is fairly easy to troubleshoot any faults. Less prone to bottlenecks than bus topology, as large amounts of traffic are not travelling across the network at anyone time | Inefficient because it may have to visit many multiple devices first before reaching the intended device. Fault such as a cut cable or broken device will result in the entire networking breaking |

### Switch:

Are dedicated **devices** within a **network** that are designed to aggregate multiple other **devices** such as computers, printers, or any other networking-capable device using ethernet, where they plug into a switchs port.

Switches are usually found in larger networks such as businesses, school, or similair sized networks, where there are mayn **devices** to connect to the network.

Additionally, **switches** are much more efficient than their lesser counterpart (**hubs** / **repeaters**).
They keep track of what device is connected to which part.
This way, when they receive a packet, instead of repeating that packet to every port like a **hub** would do, it just sends it to the intended target, thus reducing **network** traffic.

![Switches](https://raw.githubusercontent.com/MathewHDYT/OneShare/main/_images/switches.png)

### Router:

It's job it to connect **networks** and pass data between them, it does so by **routing**.

**Routing** is the label given to the process of data travelling across **networks**. It involves creating a path between **networks** so that this data can be successfully delivered. 

**Routing** is useful when **devices** are connected by many paths. 

![Routing](https://raw.githubusercontent.com/MathewHDYT/OneShare/main/_images/routing.png)

### Subnetting:

Is the term given to splitting up a **network** into smaller, miniature networks within itself.

For example a business with different departments such as:
- Accounting
- Finance
- Human Resources 

![Subnet](https://raw.githubusercontent.com/MathewHDYT/OneShare/main/_images/subnet.png)

A **network** needs to know which department to send the information too.
Therefore, **network** administrators use subnetting to categorize and assign specific parts of a **network**.

That is done by splitting up the number of hosts that can fit within the **network**, represented by a number called a **subnet mask**.

A subnet like a **`IP` address** is made up of four sections called octets as a number of four bytes (32 bits), ranging from 0 to 255.

**Subnets** use **`IP` address** in three different ways:
- Identify the **network address**
- Identify the **host address**
- Identify the **default gateway**

**TYPE** | **PURPOSE** | **EXPLANATION** | **EXAMPLE** |
Network Address | This address identifies the start of the actual **network** and is used to identify a **networks** existence | For example, a **device** with the **`IP` address** of 192.168.1.100 will be on the network identified by 192.168.1.0 | 192.168.1.0 |
Host Address | An **`IP` address** here is used to identify a **device** on the **subnet** | For example, a **device** will have the network address of 192.168.1.1 | 192.168.1.100 |
Default Gateway | The default gateway address is a special address assigned to a **device** on the network that is capable of sending information to another **network** | Any data that needs to go to a device that isn't on the same **network** (i.e. isn't on 192.168.1.0) will be sent to this **device**. These devices can use any host address but usually use either the first or last host address in a **network** (.1 or .254) | 192.168.1.254 |

In small **networks**, creating **subnets** is not needed as there is an unlikely chance that you need more than 254 **devices** connected at one time. 

However, in bigger **networks** subnetting can provide a range of benefits including:
- Efficiency
- Security
- Full control 

### ARP Protocol:

`Adress Resolution Protocol` is responsible for allowing devices to identify themselves on a network. It does that with allowing **devices** to associate their `MAC` address with a **`IP` address** on the **network**.
Each **device** will additionally keep a log of the `MAC` addresses associated with other devices.

When **devices** then wish to communicate with another, they will send a broadcast to the entire network searching for a specific **device**.
These **devices** can then use the `ARP` protocol to find the `MAC` address of a device for communication.

### How does ARP Work? 

Each **device** in a **network** has a ledger to store information on (cache). With the `ARP` protocol, this cache stores the identifiers of other **devices** on the **network**.

To map these two identifiers the `ARP` protocol sends two types of messages:
- `ARP` Request
- `ARP` Reply

When a `ARP` request is sent, a message is broadcasted to every other **device** found on a **network** by the **device**, asking whether or not the **devices** `MAC` address matches the requested **`IP` address**.

If the **device** does have the requested **`IP` address**, an `ARP` reply is returned to the initial **device**, which then remembers this information within the cache (`ARP` entry).

![ARP](https://raw.githubusercontent.com/MathewHDYT/OneShare/main/_images/arp.png)

### DHCP Protocol:

**`IP` addresses** can be assigned either manually, by entering them physically into a device, or automatically and most commonly by using a `DHCP` (`Dynamic Host Configuration Protocol`) server.

This works when a **device** connects to a **network** and if the **`IP` address** has not been manually assigned already, sends out a request (`DHCP` Discover) to see if any `DHCP` servers are on the **network**.

If there are, the `DHCP` server replies with a **`IP` address** the **device** could use (`DHCP` Offer).
The **device** then sends a reply confirming it wants the offered **`IP` address** (`DHCP` Request).

Lastly the `DHCP` server send a reply acknowledging this has been completed, and the **device** can start using the **`IP` address** (`DHCP` ACK).

![DHCP](https://raw.githubusercontent.com/MathewHDYT/OneShare/main/_images/dhcp.png)
