# Computer Networking
---

# 1. What is Computer Networking?

Computer networking is the communication between two or more devices over a network using agreed-upon **protocols/rules**.

Examples of networking protocols:

* HTTP / HTTPS
* DNS
* TCP / UDP
* IP
* Ethernet / Wi-Fi

### Important distinction

The **OSI model is not itself a set of networking rules**.

It is a **7-layer conceptual model** that helps us understand how networking works.

---

# 2. OSI Model

The OSI model has 7 layers:

| Layer | Name         | Examples / Purpose                |
| ----- | ------------ | --------------------------------- |
| 7     | Application  | HTTP, DNS, SMTP                   |
| 6     | Presentation | Encoding, encryption, compression |
| 5     | Session      | Session management                |
| 4     | Transport    | TCP, UDP                          |
| 3     | Network      | IP, routing                       |
| 2     | Data Link    | Ethernet, Wi-Fi                   |
| 1     | Physical     | Cables, radio signals             |

### HLD focus

For software engineering / HLD, initially focus mainly on:

**Application → Transport → Network**

Know the other layers at a high level.

Simplified:

```text
Application
HTTP / HTTPS / DNS
        ↓
Transport
TCP / UDP
        ↓
Network
IP / Routing
        ↓
Data Link
Ethernet / Wi-Fi
        ↓
Physical
Actual signals
```

---

# 3. What Happens When I Type a URL?

Suppose you enter:

```text
https://www.example.com
```

A simplified flow is:

```text
User enters URL
      ↓
DNS lookup
      ↓
Domain name → IP address
      ↓
TCP connection
      ↓
TLS handshake
      ↓
HTTP request
      ↓
Server processes request
      ↓
HTTP response
```
First, we use DNS to convert a human-readable domain name into an IP address like 32.42.52.62. Then, a series of carefully orchestrated steps begins. We set up a TCP connection over IP, send our HTTP request, get a response, and tear down the connection.

---

# 4. DNS

**DNS = Domain Name System**

DNS maps a domain name such as:

```text
www.example.com
```

to one or more IP addresses.

Think of DNS as a **distributed naming / lookup system**, rather than simply:

> "Converting English characters into an IP."

Example:

```text
www.example.com
       ↓
      DNS
       ↓
IP address
```

The returned address may be:

```text
IPv4
```

or

```text
IPv6
```

or both.

DNS provides name → address resolution
DNS is distributed and hierarchical
DNS responses can be cached
A domain can resolve to multiple IPs
DNS can be used for traffic distribution and routing decisions

Example:

Client
  ↓
DNS
  ↓
Load Balancer / Server IP

---

# 5. TCP Connection Establishment

TCP establishes a reliable connection using a **3-way handshake**.

```text
Client                         Server

   SYN  --------------------->

        <---------------------  SYN + ACK

   ACK  --------------------->

       TCP connection established
```

### Meaning

### 1. SYN

Client says:

> I want to establish a TCP connection.

### 2. SYN + ACK

Server says:

> I acknowledge your request and synchronize my side.

### 3. ACK

Client acknowledges the server.

The TCP connection is now established.

After this, application data can be exchanged.

---

# 6. TCP Connection Termination

TCP connection termination is commonly described as a **4-way handshake**.

```text
Client                         Server

   FIN  --------------------->

        <---------------------  ACK

        <---------------------  FIN

   ACK  --------------------->
```

Packets:

* **FIN** → Finish
* **ACK** → Acknowledgment

### Why 4 messages?

TCP is **full-duplex**.

The two directions of communication can be closed independently.

Conceptually:

```text
Client → Server
```

and

```text
Server → Client
```

can be closed separately.

---

# 7. Network Layer

The Network layer is primarily concerned with:

* IP addressing
* Routing packets between networks

Simplified:

```text
Application
     ↓
Transport
TCP / UDP
     ↓
Network
IP / Routing
```

The Network layer answers:

> Where should this packet go?

Routers primarily operate at this layer.

---

# 8. What is an IP Address?

An IP address is a network-layer address used to identify an **endpoint/interface and enable communication and routing between networks.**

There are two major versions:

* IPv4
* IPv6

Important:

> IP addresses are not necessarily permanent identifiers of a physical device.

They can change depending on the network the device is connected to.

For example:

```text
Laptop

Home Wi-Fi:
192.168.1.10

Office Wi-Fi:
10.20.5.18
```

Same laptop, different IP addresses.

---

# 9 IPv4 and Ipv6

An IP address identifies a network interface/address endpoint for communication and enables routing between networks.

Two versions:

``` text
IPv4 → 32-bit, ~4.3 billion addresses
IPv6 → 128-bit, vastly larger address space
```

An IP is not necessarily a permanent identity of a physical device. It can change when the device/network changes.

Private vs Public

Private IP

Used inside private networks
Not globally routable on the public Internet
Examples: 10.x.x.x, 172.16.x.x – 172.31.x.x, 192.168.x.x

Public IP

Globally routable Internet address

Important:

Private/Public ≠ IPv4/IPv6

These are different concepts.