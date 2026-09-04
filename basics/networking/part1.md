# Computer Networking — Complete HLD Foundation

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

For a typical HTTPS request:

```text
DNS
 ↓
TCP handshake
 ↓
TLS handshake
 ↓
HTTP request
 ↓
HTTP response
```

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

An IP address identifies a **network interface within an IP network**.

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

# 9. IPv4

IPv4 uses **32-bit addresses**.

Therefore its theoretical address space is:

```text
2³²
≈ 4.3 billion addresses
```

Example:

```text
192.168.1.10
```

IPv4 is usually written as four decimal numbers:

```text
192 . 168 . 1 . 10
```

Each part represents 8 bits:

```text
8 + 8 + 8 + 8 = 32 bits
```

---

# 10. Private IPv4 Addresses

A **private IP address** is used inside a private/local network.

Common private IPv4 ranges:

```text
10.0.0.0        – 10.255.255.255
172.16.0.0      – 172.31.255.255
192.168.0.0     – 192.168.255.255
```

Example:

```text
Laptop → 192.168.1.10
Phone  → 192.168.1.20
TV     → 192.168.1.21
```

These addresses are **not globally routable on the public Internet**.

They are commonly used inside:

* Home networks
* Office networks
* Private cloud networks
* VPCs

---

# 11. Public IP Address

A **public IP address** is an address that can be globally routed across the Internet.

Example:

```text
49.x.x.x
```

A typical home network:

```text
Laptop
192.168.1.10
      \
       \
Phone   → Wi-Fi Router → Public IP → Internet
192.168.1.20              49.x.x.x
```

The router may receive a public IP through the ISP's network.

So:

```text
Private IP
→ local/private network

Public IP
→ Internet/global connectivity
```

---

# 12. NAT

**NAT = Network Address Translation**

NAT allows private IPv4 addresses to communicate through public IPv4 addresses.

Example:

```text
Laptop
192.168.1.10:50000
       ↓
     Router
       ↓
      NAT
       ↓
49.x.x.x:62001
       ↓
   Internet
       ↓
   Web Server
```

The server generally sees the public-side address:

```text
49.x.x.x
```

rather than:

```text
192.168.1.10
```

### Why is NAT heavily used with IPv4?

IPv4 addresses are limited.

Instead of:

```text
Laptop → unique public IPv4
Phone  → unique public IPv4
TV     → unique public IPv4
Camera → unique public IPv4
```

we can have:

```text
Laptop ─┐
Phone  ─┤
TV     ─┤
Camera ─┘
         ↓
      Router
         ↓
   One public IPv4
         ↓
      Internet
```

Many devices can therefore share one public IPv4 address.

---

# 13. Important: Private/Public vs IPv4/IPv6

These are **different concepts**.

### Private/Public

Describes the intended scope of an address:

```text
Private/local
vs
Public/global
```

### IPv4/IPv6

Describes the **Internet Protocol version**:

```text
IPv4
vs
IPv6
```

Therefore:

```text
IPv4 can be private
IPv4 can be public

IPv6 can have globally routable addresses
IPv6 also has non-global address types
```

Do NOT think:

```text
IPv4 = private
IPv6 = public
```

That is incorrect.

---

# 14. Why Do We Need IPv6?

The biggest reason:

## IPv4 has a limited address space.

IPv4:

```text
32 bits
↓
2³²
≈ 4.3 billion addresses
```

The number of Internet-connected devices grew enormously:

* Smartphones
* Laptops
* Servers
* Cloud infrastructure
* IoT devices
* Smart TVs
* Cameras
* Cars
* Sensors
* etc.

The IPv4 address space became insufficient.

This led to the development and deployment of:

**IPv6**

---

# 15. IPv6

IPv6 uses **128-bit addresses**.

Therefore:

```text
2¹²⁸
≈ 3.4 × 10³⁸ addresses
```

This is vastly larger than IPv4.

Example:

```text
2001:db8:85a3::8a2e:370:7334
```

IPv6 addresses are generally represented using hexadecimal.

### Core difference

```text
IPv4
32-bit
~4.3 billion addresses

IPv6
128-bit
~3.4 × 10³⁸ addresses
```

The key thing to remember:

> **IPv6 provides a vastly larger address space.**

---

# 16. Does IPv6 Eliminate NAT?

IPv6 has such a large address space that devices can have globally routable addresses without requiring NAT **for address conservation**.

IPv4 commonly looks like:

```text
Many devices
     ↓
Private IPv4 addresses
     ↓
NAT
     ↓
Public IPv4
     ↓
Internet
```

IPv6 can support:

```text
Device
   ↓
Globally routable IPv6 address
   ↓
Internet
```

But:

> **Globally routable does NOT mean publicly accessible.**

Firewalls and security rules can still block incoming traffic.

---

# 17. IPv4 and IPv6 Can Coexist

IPv6 did not replace IPv4 overnight.

Many networks support both.

This is called **dual stack**.

```text
                    Device
                   /      \
                IPv4      IPv6
                  ↓         ↓
               Internet  Internet
```

A server may have:

```text
IPv4: 142.x.x.x

IPv6: 2001:db8::1234
```

A client may have IPv4 connectivity, IPv6 connectivity, or both.

---

# 18. Who Gives a Device Its IP Address?

This is an important mental-model question.

There is **not one central organization that directly assigns an IP to every device**.

IP allocation happens at different levels.

```text
Global Internet
       ↓
IANA / Regional Internet Registries
       ↓
ISP / Cloud Provider / Organization
       ↓
Network / Subnet
       ↓
Device
```

The exact mechanism depends on the network.

---

# 19. Home Wi-Fi: Who Gives My Laptop Its IP?

Suppose:

```text
Laptop connects to home Wi-Fi
```

The Wi-Fi router commonly runs **DHCP**.

**DHCP = Dynamic Host Configuration Protocol**

The router's DHCP service can assign the laptop a private IP:

```text
Laptop
   ↓
DHCP request
   ↓
Wi-Fi Router
   ↓
192.168.1.10
```

Other devices may get:

```text
Laptop → 192.168.1.10
Phone  → 192.168.1.20
TV     → 192.168.1.21
```

So:

> **In a typical home network, the router's DHCP service assigns local/private IP addresses to devices.**

---

# 20. Who Gives the Router Its Public IP?

The router's Internet connectivity comes from the **ISP**.

Conceptually:

```text
ISP
 ↓
Public Internet connectivity
 ↓
Home Router
 ↓
Public IP
```

Then:

```text
Home Router
     ↓
DHCP
     ↓
Laptop / Phone / TV
     ↓
Private IPs
```

So there are two different allocation processes:

```text
ISP
 ↓
Router / home network
 ↓
Public Internet address/connectivity


Router's DHCP
 ↓
Individual devices
 ↓
Private IP addresses
```

---

# 21. Who Allocates Global IP Addresses?

At a very high level, Internet address allocation is hierarchical.

```text
IANA
 ↓
Regional Internet Registries (RIRs)
 ↓
ISPs / Organizations / Cloud Providers
 ↓
Their networks
 ↓
Devices
```

IANA coordinates global Internet number resources.

Regional Internet Registries manage address resources for their respective regions.

For example:

```text
APNIC
```

serves the Asia-Pacific region.

You don't need to memorize the complete administrative details for HLD.

The important idea is:

> **Public IP address allocation is hierarchical.**

---

# 22. Mobile Data: Who Gives the Phone Its IP?

Mobile networking looks more like:

```text
Phone
  ↓
Cell Tower
  ↓
Mobile Carrier Network
  ↓
Internet
```

The mobile carrier manages the phone's IP connectivity.

The phone may receive:

* IPv4
* IPv6
* Both

With IPv4, carriers commonly use **CGNAT**.

**CGNAT = Carrier-Grade NAT**

Example:

```text
Phone A ─┐
Phone B ─┤
Phone C ─┤
Phone D ─┘
          ↓
   Mobile Carrier
          ↓
       CGNAT
          ↓
 Shared Public IPv4
          ↓
       Internet
```

Therefore:

> The public IPv4 address seen by a server may belong to the carrier's NAT infrastructure rather than uniquely identifying your phone.

---

# 23. What About an AWS Server?

This is the same fundamental concept, but AWS operates the networking infrastructure.

Suppose you create an EC2 instance.

Conceptually:

```text
AWS
 ↓
VPC
 ↓
Subnet
 ↓
EC2 Instance
```

For example:

```text
VPC:
10.0.0.0/16

Subnet:
10.0.1.0/24

EC2:
10.0.1.25
```

AWS networking assigns the EC2 instance a **private IP address** from the subnet's address range.

So:

```text
AWS VPC
   ↓
Subnet
   ↓
AWS networking
   ↓
EC2
   ↓
Private IP
10.0.1.25
```

You can also configure specific private IPs in supported scenarios.

---

# 24. What About the AWS Server's Public IP?

An EC2 instance does not inherently have to be directly exposed to the Internet.

It can have:

```text
Private IP only
```

or:

```text
Private IP
    +
Public IP
```

For example:

```text
EC2
Private IP: 10.0.1.25
       │
       │ AWS networking
       ↓
Public IP: 3.x.x.x
       │
       ↓
Internet
```

AWS manages the public IP address from its available Internet address resources.

For a stable public IPv4 address, AWS provides **Elastic IP addresses**.

---

# 25. Who Ultimately Gave AWS Its Public IPs?

AWS doesn't randomly invent public Internet IP addresses.

At a very high level:

```text
IANA
 ↓
Regional Internet Registry
 ↓
AWS / Amazon
 ↓
AWS networking infrastructure
 ↓
VPC / subnet / service
 ↓
Public IP
```

AWS receives/manages Internet address resources through the Internet's hierarchical address-allocation system.

AWS then manages those addresses across its infrastructure.

So the key distinction is:

```text
Internet-level allocation
        ↓
AWS gets address resources
        ↓
AWS manages them internally
        ↓
AWS assigns/associates addresses
to its networking resources
```

---

# 26. AWS Private IP vs Public IP

An EC2 instance might look like:

```text
                 Internet
                    │
                    │
              Public IP
               3.x.x.x
                    │
                    ▼
             AWS networking
                    │
                    ▼
             Private IP
              10.0.1.25
                    │
                    ▼
                 EC2
```

But in production, we often don't expose EC2 instances directly.

Instead:

```text
Internet
   ↓
Public Load Balancer
   ↓
Private EC2 instances
   ↓
Database
```

This is a very common HLD architecture.

The application servers can remain on private networks while only the load balancer is Internet-facing.

---

# 27. MAC Address vs IP Address

Another useful distinction:

A network interface typically has a **MAC address** associated with it.

Conceptually:

```text
Network Interface
       │
       ├── MAC address
       │
       └── IP address
```

MAC operates primarily at the **Data Link layer**.

IP operates at the **Network layer**.

An IP address can change:

```text
Home Wi-Fi:
192.168.1.10

Office Wi-Fi:
10.20.5.18
```

while the network interface can still have the same MAC address.

Therefore:

> **MAC address ≠ IP address**

They serve different purposes.

---

# 28. Complete Example — Home Wi-Fi

Suppose you have:

```text
Laptop
Private IP:
192.168.1.10
```

Your home router might have:

```text
Public IP:
49.x.x.x
```

The architecture is:

```text
Laptop
192.168.1.10
      ↓
Wi-Fi
      ↓
Home Router
      ↓
NAT
      ↓
49.x.x.x
      ↓
Internet
      ↓
Web Server
```

---

# 29. Complete Example — Mobile Data

```text
Phone
   ↓
Cell Tower
   ↓
Mobile Carrier
   ↓
CGNAT
   ↓
Public IPv4
   ↓
Internet
   ↓
Server
```

Or with IPv6:

```text
Phone
   ↓
Cell Tower
   ↓
Mobile Carrier
   ↓
IPv6 connectivity
   ↓
Internet
   ↓
Server
```

---

# 30. Complete Example — AWS

```text
                    Internet
                       │
                       ↓
                Public IP / DNS
                       │
                       ↓
                Load Balancer
                       │
                       ↓
                 AWS VPC
                       │
                       ↓
                 Private Subnet
                       │
              ┌────────┴────────┐
              ↓                 ↓
           EC2 #1             EC2 #2
         10.0.1.10           10.0.1.11
              │                 │
              └────────┬────────┘
                       ↓
                    Database
```

The important architecture idea:

> **Not every server needs a public IP.**

Often only the Internet-facing component needs one.

---

# 31. Complete Networking Mental Model

At the software/HLD level:

```text
                     APPLICATION
                 HTTP / HTTPS / DNS
                          ↓
                     TRANSPORT
                      TCP / UDP
                          ↓
                      NETWORK
                    IPv4 / IPv6
                    IP / Routing
                          ↓
                     DATA LINK
                  Ethernet / Wi-Fi
                          ↓
                     PHYSICAL
                   Actual signals
```

For Internet communication:

```text
Device
  ↓
Local IP
  ↓
Router / Carrier / Cloud Network
  ↓
NAT / CGNAT
  ↓
Public IP (many IPv4 cases)
  ↓
Internet
  ↓
Destination IP
  ↓
Server / Load Balancer
```

---

# 32. Who Allocates What?

This is the simplest way to remember the entire IP allocation picture:

```text
                 GLOBAL INTERNET
                       │
                 IANA / RIRs
                       │
          ┌────────────┼────────────┐
          ↓            ↓            ↓
         ISP          AWS        Other orgs
          │            │
          ↓            ↓
       Network        VPC
          │            │
          ↓          Subnet
       Router           │
          │             ↓
       DHCP           EC2
          │             │
          ↓             ↓
      Laptop/Phone   Private IP
      Private IP
```

For public addresses:

```text
IANA / RIR
     ↓
ISP / AWS / Organization
     ↓
Public Internet address resources
```

For local/private addresses:

```text
Local network / VPC
     ↓
DHCP / network configuration
     ↓
Device
```

---

# 33. One Final Mental Picture

If you remember only one diagram, remember this:

```text
                              USER
                                │
                                ▼
                         Browser / App
                                │
                                ▼
                               DNS
                       "Where is the server?"
                                │
                                ▼
                         IPv4 / IPv6
                       "What is its address?"
                                │
                                ▼
                      ┌──────────────────┐
                      │ Transport Layer  │
                      │    TCP / UDP     │
                      └──────────────────┘
                                │
                                ▼
                         Internet Routing
                                │
                 ┌──────────────┴──────────────┐
                 │                             │
             Home Wi-Fi                   Mobile Data
                 │                             │
             Private IP                   Carrier Network
                 │                             │
                NAT                          CGNAT
                 │                             │
             Public IP                    Shared Public IP
                 │                             │
                 └──────────────┬──────────────┘
                                │
                                ▼
                             Internet
                                │
                                ▼
                         Server / Load Balancer
                                │
                                ▼
                           Application
                                │
                                ▼
                            Response
```

---

# 34. What You Should Remember for HLD Interviews

You don't need to memorize every networking detail.

Be able to explain these clearly:

### 1. DNS

> Converts a domain name into IP address information through a distributed lookup system.

### 2. IP

> Provides addressing and routing between networks.

### 3. IPv4

> 32-bit address space, approximately 4.3 billion addresses.

### 4. IPv6

> 128-bit address space, created primarily to provide a vastly larger address space.

### 5. Private IP

> Used within local/private networks and is not globally routable on the public Internet.

### 6. Public IP

> Globally routable Internet address.

### 7. NAT

> Translates private IPv4 traffic to public IPv4, allowing many devices to share public addresses.

### 8. DHCP

> Automatically provides network configuration, commonly including private IP addresses, to devices on a local network.

### 9. CGNAT

> Carrier-level NAT that allows many mobile/broadband customers to share public IPv4 addresses.

### 10. AWS

> AWS manages IP addresses within its networking infrastructure. An EC2 instance commonly gets a private IP from its VPC subnet and may additionally have a public IP if required.

### 11. TCP

> Reliable connection-oriented transport using a 3-way handshake to establish a connection.

### 12. HTTPS

> HTTP communication secured using TLS.

### 13. Typical HTTPS flow

```text
DNS
 ↓
TCP
 ↓
TLS
 ↓
HTTP
```

---

# 35. The Core Mental Model

The most important thing to internalize is:

```text
WHO NEEDS TO COMMUNICATE?
        ↓
WHAT IS THE DESTINATION?
        ↓
DNS → IP
        ↓
HOW DOES THE PACKET GET THERE?
        ↓
IP + Routing
        ↓
HOW IS DATA TRANSPORTED?
        ↓
TCP / UDP
        ↓
HOW IS IT SECURED?
        ↓
TLS
        ↓
WHAT IS THE APPLICATION REQUEST?
        ↓
HTTP
```

And for IP allocation:

```text
Global address resources
        ↓
ISP / Cloud Provider
        ↓
Network / VPC / Subnet
        ↓
DHCP / Network configuration
        ↓
Device / Server
        ↓
IP address
```

**This is the networking foundation you should carry forward into HLD.**
