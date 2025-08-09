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

### Purpose of Headers

Headers are metadata attached to packets, akin to an envelope containing a letter. They instruct network devices on how to process and route the packet. Without headers, network infrastructure wouldn't know how to handle the data.

### Standardization of Headers

Headers function as the interface between end hosts and the network infrastructure. For interoperability, all devices must adhere to standardized header formats. Modifying these formats is challenging and requires consensus across the internet.

### Components of a Header

A typical header includes:

- **Destination Address**: Indicates where the packet should go.
    
- **Source Address**: Identifies the sender.
    
- **Checksum**: Detects errors in the packet.
    
- **Packet Length**: Specifies the size of the packet.
    

While not always mandatory, these fields are commonly included for efficient and reliable communication.

### Layered Header Structure

In networking, data is encapsulated in multiple layers, each adding its own header:

- **Application Layer**: Adds the application-specific header.
    
- **Transport Layer**: Adds a transport header (e.g., TCP/UDP).
    
- **Internet Layer**: Adds an IP header.
    
- **Link Layer**: Adds a link-specific header.
    

Each layer processes its respective header, ensuring data is properly routed and delivered.

### Addressing and Naming

Network addresses identify the location of devices within the network. These addresses are crucial for routing packets to their correct destinations.

### Header Processing at Hosts and Routers

When a host sends data, it adds headers for each layer. As the data traverses the network, routers strip off the headers of the layers they handle and add new headers appropriate for the next hop. This process continues until the data reaches its destination, where headers are removed in reverse order.


# Network Architecture

- **Design Paradigms**: The Internet's architecture is influenced by several key design principles:
    
    - **Federation**: Independent operators cooperate to form a global network.
    - **Scalability**: The system is designed to handle a vast number of users and devices.
    - **End-to-End Principle**: Certain features are best implemented at the endpoints of communication systems.
    - **Narrow Waist**: The Internet's architecture has a "narrow waist" at the IP layer, allowing for flexibility in upper and lower layers.
    - **Demultiplexing**: Differentiating traffic for multiple applications using ports and sockets.
- **Key Concepts**:
    
    - **Autonomous Systems (ASes)**: Independent networks that operate under a single administrative domain.
    - **Routing**: Determining the best paths for data transmission across networks.
    - **Protocols**: Agreed-upon rules that govern data exchange, such as TCP/IP.

---

# Designing Resource Sharing

- **Statistical Multiplexing**: Dynamic allocation of network resources based on demand, allowing multiple users to share the same bandwidth efficiently.
    
- **Circuit Switching vs. Packet Switching**:
    
    - **Circuit Switching**: Establishes a dedicated communication path between two endpoints for the duration of the connection.
    - **Packet Switching**: Data is divided into packets and sent independently over the network, allowing for more efficient use of resources.
- **Trade-offs**:
    
    - **Circuit Switching**: Provides consistent performance but can be inefficient during periods of low usage.
    - **Packet Switching**: More efficient but can lead to variable performance due to network congestion.

---

# Links

- **Properties of Links**:
    
    - **Bandwidth**: The maximum rate at which data can be transmitted over a link.
    - **Propagation Delay**: The time it takes for a signal to travel from the sender to the receiver.
    - **Bandwidth-Delay Product (BDP)**: The product of bandwidth and propagation delay, indicating the amount of data that can be in transit in the network at any given time.
- **Packet Delay**: The total time it takes for a packet to travel from the sender to the receiver, including transmission and propagation delays.
    
- **Timing and Pipe Diagrams**:
    
    - **Timing Diagram**: Illustrates the sequence of events over time during data transmission.
    - **Pipe Diagram**: Represents the flow of data as it moves through the network, helping to visualize delays and throughput.
- **Overloaded Links**: When a link's capacity is exceeded, leading to congestion and potential packet loss.
    

---