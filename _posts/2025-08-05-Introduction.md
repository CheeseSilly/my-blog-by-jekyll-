---
title: 2025/8/6 Introduction to the Internet
author: Sillycheese
date: 2025/8/6 20:19:53 +0800
categories:
  - CS168
---
# Layers of the Internet

## Physical Layer

In the Internet, we’re looking for a way to signal bits (1s and 0s) across space. The technology could be voltages on an electrical wire, wireless radio waves, light pulses along optical fiber cables, among others. There are entire fields of electrical engineering dedicated to sending signals across space, but we won’t go into detail in this class.

## Link layer

a link connect two machines.If we use links to connect up a bunch of nearby computers (e.g. all the computers in UC Berkeley), we get a **local area network (LAN)**.

we can also group bits into units of data called **packets**(or frames),and define where a packet starts and ends in the physical signal. also need to handle parallelism problems like using the same wire to send data.

## Internet Layer

what if two people in different areas wanted to communicate? And what if the two local networks were in different continents?

Instead, a smarter approach would be to introduce a post office in each network, and just connect the two post offices together.

In Internet, it is called a **switch** or **route**

## Network of Networks

internet is often described as a network of networks.

In the network, different links might be using different Layer 2 technology. Some links might use wired Ethernet, and other links might use optical fiber or wireless cellular technology. At Layer 2, we figure out how to send a packet inside a local network, across the link(s) in that network, using the specific technology in that network. Then, at Layer 3, we use the ability to send packets along links as a building block to send packets anywhere in the Internet. As the packet hops across different networks, it may be transmitted across lots of different types of links.

## Layers of Abstraction

“Modularity based on abstraction is the way things are done.” (Barbara Liskov, Turing lecture).

## Best-Effort Service Model

the Internet only supports **best effort** delivery of data.But no guarantee that the data will be delivered. Also won't tell you whether or nor the delivery succeeded.

## Packets Abstraction

 the primary unit of data transfer at Layer 3 is a **packet**, which is some small chunk of bytes that travel through the Internet, bouncing between routers, as a single unit.

## Transport

This layer uses Layer 3 as a building block, and implements an additional protocol for re-sending lost packets, splitting data into packets, and reordering packets that arrive out-of-order (among other features).

this protocol can allows us to stop thinking in terms of packets, and start thinking in terms of flows,streams of packets that are exchanged between two endpoints.

## Application

it is being built on the top of the Internet.

focus on the infrastructure supporting the application layer.

# Headers


