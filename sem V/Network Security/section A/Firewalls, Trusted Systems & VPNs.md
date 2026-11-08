# Firewalls, Trusted Systems & VPNs
---

# 1. Firewall

## 1.1 What is a Firewall?

A **firewall** is a security mechanism that controls traffic flowing between two or more networks according to a predefined security policy.

Most commonly, a firewall is placed between:

- A **trusted internal network** (e.g., company LAN)
    
- An **untrusted external network** (e.g., Internet)
    

```text
                 INTERNET
                    │
             Untrusted Network
                    │
                    ▼
             ┌─────────────┐
             │  FIREWALL   │
             └─────────────┘
                    │
                    ▼
             Trusted Network
          ┌──────────────────┐
          │ Company LAN      │
          │                  │
          │ PC  Server  DB   │
          └──────────────────┘
```

The firewall examines network traffic and decides whether to:

- **Allow** it
    
- **Block/deny** it
    
- **Log** it
    
- Sometimes **modify** it
    

### Simple example

Suppose a company wants:

> Employees can access websites, but people on the Internet cannot directly access internal computers.

The firewall can implement:

```text
Internal → Internet       ALLOW
Internet → Internal       DENY
```

However, firewalls can implement much more complicated rules.

---

# 2. Why Do We Need Firewalls?

A network connected to the Internet is exposed to many threats.

Examples include:

- Unauthorized access
    
- Malware
    
- Port scanning
    
- Denial-of-Service attacks
    
- Exploitation of vulnerable services
    
- Unauthorized remote connections
    
- Data exfiltration
    
- Network reconnaissance
    

A firewall acts as a **barrier/control point** between networks.

It reduces the attack surface by preventing unnecessary communication.

For example, if a company's database server only needs to communicate with an application server, the firewall can block all other traffic.

```text
Internet
   │
   ▼
Firewall
   │
   ├── Web Server       ← Allow HTTP/HTTPS
   │
   ├── Application      ← Restricted
   │
   └── Database         ← Only app server allowed
```

---

# 3. Firewall Design Principles

Firewall design is based on several important security principles.

## 3.1 All traffic must pass through the firewall

The firewall should be positioned so that traffic between the protected and unprotected networks cannot bypass it.

```text
             Internet
                │
                ▼
            Firewall
                │
                ▼
          Internal Network
```

If there is another connection:

```text
Internet ───────────────┐
                        │
                        ▼
                 Internal Network
                        ▲
                        │
                    Firewall
```

the attacker could bypass the firewall entirely.

Therefore:

> **All traffic between the protected network and external network should pass through the firewall.**

---

# 4. Firewall Design Principle: Traffic Must Be Authorized

The firewall should examine traffic and determine whether it is permitted by the organization's security policy.

For example:

|Source|Destination|Protocol|Port|Action|
|---|---|---|---|---|
|Internal LAN|Internet|TCP|443|Allow|
|Internet|Web Server|TCP|443|Allow|
|Internet|Internal PC|TCP|3389|Deny|
|Internet|Database|TCP|3306|Deny|

The rules represent the organization's **security policy**.

---

# 5. Firewall Design Principle: Secure the Firewall Itself

The firewall is a critical security component.

If an attacker compromises the firewall, they may gain control over the network's security boundary.

Therefore:

- Firewall software should be patched.
    
- Administrative access should be restricted.
    
- Strong authentication should be used.
    
- Unnecessary services should be disabled.
    
- Firewall configuration should be protected.
    
- Firewall logs should be monitored.
    
- Administrative interfaces should not be exposed unnecessarily.
    

---

# 6. Firewall Design Principle: Fail-Safe Defaults

A common security principle is:

> **Deny by default; allow only what is explicitly permitted.**

For example:

```text
Default = DENY

Rule 1: Allow HTTPS
Rule 2: Allow DNS
Rule 3: Allow approved VPN
Everything else → DENY
```

This is safer than:

```text
Default = ALLOW
Block only known threats
```

because unknown traffic could otherwise pass through.

This is called the **default deny principle**.

---

# 7. Firewall Design Principle: Least Privilege

Users and systems should receive only the network access they actually need.

For example:

A database server does not need to be accessible from the entire Internet.

Instead:

```text
Internet
   │
   X
Database

Application Server
       │
       ▼
   Database
```

Only the application server is permitted to communicate with the database.

This reduces the consequences of a compromised machine.

---

# 8. Firewall Design Principle: Defense in Depth

A firewall should not be the **only** security mechanism.

A secure network may use:

```text
Internet
   │
   ▼
Firewall
   │
   ▼
IDS/IPS
   │
   ▼
Web Application Firewall
   │
   ▼
Servers
   │
   ▼
Endpoint Security
```

If one security mechanism fails, another can provide protection.

This is known as **defense in depth**.

---

# 9. Characteristics of a Firewall

A good firewall generally has the following characteristics.

## 9.1 Access Control

It determines which traffic is permitted.

Example:

```text
Allow:
TCP 443
TCP 80

Block:
TCP 23
TCP 3389 from Internet
```

---

## 9.2 Traffic Monitoring

Firewalls can monitor network traffic and maintain logs.

Example:

```text
Source: 192.168.1.25
Destination: 8.8.8.8
Protocol: UDP
Port: 53
Action: ALLOWED
```

Logs can help identify:

- Attacks
    
- Suspicious connections
    
- Policy violations
    
- Network problems
    

---

## 9.3 Authentication

Some firewalls can require authentication before allowing access.

For example:

```text
User
 ↓
Authentication
 ↓
Firewall
 ↓
Internal Network
```

Authentication may involve:

- Username/password
    
- Certificates
    
- Multi-factor authentication
    
- Tokens
    

---

## 9.4 Address and Port Filtering

Firewalls can filter traffic based on:

- Source IP
    
- Destination IP
    
- Source port
    
- Destination port
    
- Protocol
    

For example:

```text
Source: 10.0.0.0/24
Destination: 192.168.1.10
Port: 443
Protocol: TCP
Action: ALLOW
```

---

## 9.5 Network Address Translation

Many firewalls provide **NAT (Network Address Translation)**.

For example:

```text
Private IP              Public IP
192.168.1.10 ─────────► 203.x.x.x
```

This allows multiple internal systems to share public IP addresses.

NAT itself is **not a complete security mechanism**, although it can reduce direct exposure of internal addresses.

---

# 10. Types of Firewalls

The major firewall types are:

1. Packet-filtering firewall
    
2. Stateful inspection firewall
    
3. Circuit-level gateway
    
4. Application-level gateway / proxy firewall
    
5. Next-generation firewall
    
6. Host-based firewall
    
7. Network-based firewall
    

---

# 11. Packet-Filtering Firewall

A **packet-filtering firewall** examines individual packets and makes decisions based on packet-header information.

It can inspect:

- Source IP
    
- Destination IP
    
- Protocol
    
- Source port
    
- Destination port
    
- Interface
    

Example:

```text
Source       Destination       Port       Action
----------------------------------------------------
10.0.0.0/24  Any               443        ALLOW
Any          10.0.0.5          23         DENY
```

### Example

A packet:

```text
Source IP:      192.168.1.20
Destination IP: 8.8.8.8
Protocol:       TCP
Destination:    443
```

could be allowed because HTTPS is permitted.

### Advantages

- Simple
    
- Fast
    
- Low resource consumption
    
- Easy to implement in routers
    

### Disadvantages

- Limited understanding of connections
    
- Cannot easily understand application-level content
    
- Can be vulnerable to sophisticated attacks
    
- Stateless filtering has limited context
    

---

# 12. Stateful Inspection Firewall

A **stateful firewall** keeps track of the state of network connections.

This is an improvement over simple packet filtering.

Suppose a client sends:

```text
Client ─────── SYN ───────► Server
Client ◄──── SYN-ACK ────── Server
Client ─────── ACK ───────► Server
```

The firewall records that a valid TCP connection has been established.

When the server sends response traffic, the firewall knows that the traffic belongs to an existing connection.

### Stateful table

The firewall might maintain information such as:

|Source|Destination|Protocol|State|
|---|---|---|---|
|192.168.1.20:5000|93.x.x.x:443|TCP|ESTABLISHED|

Therefore:

> A stateful firewall understands the context of a connection, not merely individual packets.

### Advantages

- More secure than basic packet filtering
    
- Tracks connection state
    
- Blocks many unsolicited packets
    
- Better handling of TCP connections
    

### Disadvantages

- Requires more memory and processing
    
- More complex
    
- Large numbers of connections can consume resources
    

---

# 13. Circuit-Level Gateway

A **circuit-level gateway** operates at approximately the **session layer**.

It monitors TCP connections and determines whether sessions are permitted.

It generally does not inspect the actual application data deeply.

Conceptually:

```text
Client
  │
  ▼
Circuit Gateway
  │
  ▼
Server
```

It can verify things such as whether a TCP connection is properly established.

### Advantage

- Provides session-level control
    
- Less processing than deep application inspection
    

### Disadvantage

- Does not provide detailed application-level inspection
    

---

# 14. Application-Level Gateway / Proxy Firewall

An **application-level gateway**, commonly called a **proxy firewall**, operates at the application layer.

Instead of directly connecting:

```text
Client ───────────► Server
```

the client communicates with the proxy:

```text
Client ───► Proxy ───► Server
```

The proxy acts as an intermediary.

For example, an HTTP proxy can inspect HTTP requests.

```text
Client
   │
   │ HTTP request
   ▼
HTTP Proxy
   │
   │ validated request
   ▼
Web Server
```

It can inspect:

- URLs
    
- HTTP methods
    
- Headers
    
- Application commands
    
- Application data
    

### Advantages

- Deep inspection
    
- Application-specific security
    
- Can hide internal network structure
    
- Can enforce detailed policies
    

### Disadvantages

- More computationally expensive
    
- More complex
    
- May introduce latency
    
- Requires application-specific proxy functionality
    

---

# 15. Next-Generation Firewall (NGFW)

A **Next-Generation Firewall** combines traditional firewall functions with more advanced security capabilities.

It may provide:

- Stateful inspection
    
- Deep packet inspection
    
- Application identification
    
- User identification
    
- Intrusion prevention
    
- URL filtering
    
- Malware detection
    
- TLS/SSL inspection
    
- Threat intelligence integration
    

Instead of simply asking:

> "Is this TCP traffic on port 443?"

an NGFW may attempt to determine:

> "What application generated this traffic, which user is responsible, and does the traffic match known malicious behavior?"

---

# 16. Host-Based Firewall

A **host-based firewall** runs directly on an individual computer or server.

Examples:

```text
Laptop
 └── Host Firewall

Server
 └── Host Firewall
```

It controls traffic entering or leaving that specific machine.

### Advantages

- Protects individual systems
    
- Can apply application-specific rules
    
- Useful even outside the corporate network
    

### Disadvantages

- Must be configured on many devices
    
- Management becomes difficult at large scale
    
- Consumes host resources
    

---

# 17. Network-Based Firewall

A **network firewall** protects an entire network or network segment.

```text
             Internet
                 │
                 ▼
          Network Firewall
                 │
        ┌────────┴────────┐
        ▼                 ▼
      LAN 1             LAN 2
```

It is commonly deployed at:

- Network boundaries
    
- Data centers
    
- Cloud environments
    
- Internal network segments
    

---

# 18. Firewall Architecture

Common firewall architectures include:

## 18.1 Dual-Homed Host

A machine has two network interfaces.

```text
Internet
   │
   ▼
[ Firewall Host ]
   │
   ▼
Internal Network
```

One interface connects to the external network and another to the internal network.

The host performs forwarding and filtering.

---

## 18.2 Screened Host Architecture

A **screening router** is placed between the Internet and a bastion host.

```text
Internet
   │
   ▼
Screening Router
   │
   ▼
Bastion Host
   │
   ▼
Internal Network
```

The router performs packet filtering while the bastion host provides additional security services.

---

## 18.3 Screened Subnet Architecture

A **DMZ (Demilitarized Zone)** is placed between the Internet and internal network.

```text
                  INTERNET
                      │
                      ▼
                ┌──────────┐
                │ Firewall │
                └────┬─────┘
                     │
                    DMZ
             ┌───────┴───────┐
             │               │
         Web Server      Mail Server
             │
             ▼
        ┌──────────┐
        │ Firewall │
        └────┬─────┘
             │
       Internal Network
```

The DMZ contains systems that need to be reachable from the Internet.

For example:

- Public web servers
    
- Mail gateways
    
- DNS servers
    
- Reverse proxies
    

The important idea is that a compromised public server does not automatically provide direct access to the internal network.

---

# 19. Trusted Systems

## 19.1 What is a Trusted System?

A **trusted system** is a computer system designed to enforce a defined security policy and provide controlled access to resources.

The key concept is **trust**.

A system should not simply assume that a user or process is trustworthy.

Instead, it should enforce rules such as:

```text
Who are you?
       ↓
Authentication
       ↓
What are you allowed to access?
       ↓
Authorization
       ↓
Can you perform this operation?
       ↓
Access Control
```

---

# 20. Trusted Computing Base (TCB)

The **Trusted Computing Base (TCB)** is the collection of hardware, software, and firmware components responsible for enforcing the system's security policy.

Examples can include:

- Operating system security mechanisms
    
- Access-control mechanisms
    
- Authentication mechanisms
    
- Security kernel
    
- Hardware security components
    

The basic idea is:

> If the TCB is secure, it can reliably enforce the security policy.

If the TCB is compromised, the security of the entire system may be compromised.

---

# 21. Reference Monitor

One of the most important concepts in trusted systems is the **reference monitor**.

A reference monitor is a mechanism that controls access between subjects and objects.

- **Subject** = entity requesting access
    
- **Object** = resource being accessed
    

Example:

```text
User
  │
  │ "Read file A"
  ▼
Reference Monitor
  │
  ├── Is user authenticated?
  ├── Is user authorized?
  └── Is operation permitted?
          │
          ▼
       File A
```

The reference monitor should satisfy three important properties:

### 1. Complete mediation

Every access to a protected resource must be checked.

### 2. Isolation

The reference monitor itself must be protected from unauthorized modification.

### 3. Verifiability

It should be sufficiently small/simple that its correctness can be analyzed or verified.

---

# 22. Trusted Operating System

A **trusted operating system** provides strong security mechanisms for controlling access to system resources.

It may implement:

- User authentication
    
- Access control
    
- File permissions
    
- Process isolation
    
- Privilege separation
    
- Security labels
    
- Auditing
    
- Mandatory access control
    

For example, a highly sensitive system might classify data as:

```text
PUBLIC
CONFIDENTIAL
SECRET
TOP SECRET
```

and enforce rules determining which users can access which classifications.

---

# 23. Mandatory Access Control

In **Mandatory Access Control (MAC)**, access decisions are based on centrally defined security policies.

Users cannot freely change the security rules.

For example:

```text
User clearance: SECRET

File classification: TOP SECRET

Access → DENIED
```

This is commonly associated with highly secure environments.

Examples of technologies implementing forms of mandatory access control include SELinux and AppArmor, although their models differ.

---

# 24. Discretionary Access Control

In **Discretionary Access Control (DAC)**, resource owners generally have the ability to determine who can access their resources.

For example:

```text
File: report.txt

Owner: Rishav
Permissions:
Owner → Read + Write
Group → Read
Others → No access
```

Traditional Unix/Linux file permissions are a common example of DAC.

---

# 25. VPN — Virtual Private Network

A **Virtual Private Network (VPN)** creates a protected logical connection across an untrusted network such as the Internet.

For example:

```text
Office A                         Office B
Private Network                 Private Network
     │                                │
     ▼                                ▼
 VPN Gateway ═════ Internet ═════ VPN Gateway
              Encrypted Tunnel
```

The Internet is still being used, but the communication is protected using cryptographic mechanisms.

---

# 26. Why Do We Need VPNs?

Suppose an employee is working from home.

Without a VPN:

```text
Laptop ───────── Internet ───────── Company Server
```

Traffic may be exposed to various network threats depending on the protocol and network environment.

With a VPN:

```text
Laptop
  │
  ▼
VPN Client
  │
  ╞════ Encrypted Tunnel ════╡
  │                         │
Internet                 VPN Gateway
                            │
                            ▼
                      Company Network
```

The VPN can provide:

- Confidentiality
    
- Integrity
    
- Authentication
    
- Secure remote access
    

---

# 27. Basic Components of a VPN

A VPN generally involves:

### 1. VPN Client

Runs on the user's device.

```text
Laptop → VPN Client
```

### 2. VPN Server/Gateway

Terminates the VPN connection.

```text
VPN Client ─────► VPN Gateway
```

### 3. Authentication

Determines whether the user/device is allowed to establish the VPN.

### 4. Encryption

Protects data from unauthorized observation.

### 5. Tunneling

Encapsulates traffic so that it can travel through the underlying network.

---

# 28. VPN Tunneling

Suppose the original packet is:

```text
Original packet:
[IP Header][TCP Header][Data]
```

The VPN encapsulates it inside another packet:

```text
[Outer IP Header][VPN Header][Encrypted Original Packet]
```

Conceptually:

```text
Original packet
      ↓
 Encapsulation
      ↓
Encrypted tunnel packet
      ↓
Internet
      ↓
VPN Gateway
      ↓
Decapsulation
      ↓
Original packet
```

This is called **tunneling**.

---

# 29. VPN Encryption

VPN protocols can use cryptographic algorithms to protect traffic.

For example:

```text
Plaintext
   │
   ▼
Encryption + Key
   │
   ▼
Ciphertext
   │
   ▼
Internet
   │
   ▼
Decryption + Key
   │
   ▼
Plaintext
```

Encryption provides **confidentiality**.

Integrity mechanisms help ensure the packet was not modified.

Authentication mechanisms help establish that the communicating endpoint is legitimate.

---

# 30. Types of VPN

There are two major categories you should know:

1. Remote-access VPN
    
2. Site-to-site VPN
    

---

# 31. Remote-Access VPN

A remote-access VPN connects an individual user to a private network.

Example:

```text
Employee Laptop
      │
      │ VPN
      ▼
Internet
      │
      ▼
Company VPN Gateway
      │
      ▼
Company LAN
```

It is commonly used by:

- Remote employees
    
- Administrators
    
- Contractors
    
- Mobile workers
    

The user typically runs VPN client software.

---

# 32. Site-to-Site VPN

A site-to-site VPN connects entire networks.

Example:

```text
Office A                         Office B
10.1.0.0/24                      10.2.0.0/24
     │                                │
     ▼                                ▼
VPN Gateway ═════ Internet ═════ VPN Gateway
```

Users in Office A can communicate with resources in Office B through the VPN tunnel.

The individual computers may not need to run VPN software.

---

# 33. Intranet VPN

An **intranet VPN** connects different internal locations belonging to the same organization.

```text
Head Office
     │
     ╞════ VPN ════╡
     │
Branch Office
```

Example:

A company has offices in:

- Delhi
    
- Mumbai
    
- Bengaluru
    

VPNs can connect the three private networks securely over the Internet.

---

# 34. Extranet VPN

An **extranet VPN** allows authorized external organizations to access selected resources.

For example:

```text
Company
   │
   ╞════ VPN ════╡
   │
Supplier
```

The supplier does not receive unrestricted access to the company's network.

Instead, firewall and VPN policies restrict access to specific systems.

---

# 35. VPN Protocols

Important VPN technologies include:

## IPsec

**IPsec (Internet Protocol Security)** operates at the network layer and can protect IP communications.

It provides mechanisms for:

- Authentication
    
- Integrity
    
- Confidentiality
    

Two important IPsec protocols are:

### AH — Authentication Header

Provides authentication and integrity, but does not provide encryption.

### ESP — Encapsulating Security Payload

Can provide:

- Confidentiality
    
- Integrity
    
- Authentication
    

ESP is widely used for VPNs.

---

# 36. SSL/TLS-Based VPNs

Some VPN solutions use **TLS** to establish secure connections.

They are often useful for remote-access scenarios.

Conceptually:

```text
VPN Client
    │
    │ TLS-protected connection
    ▼
VPN Gateway
    │
    ▼
Private Network
```

They can be easier to deploy for remote users because they commonly operate over standard Internet connectivity.

---

# 37. VPN Advantages

### 1. Confidentiality

Encryption prevents unauthorized parties from easily reading protected traffic.

### 2. Integrity

Helps detect whether traffic has been modified.

### 3. Authentication

Helps verify users/devices or VPN endpoints.

### 4. Secure remote access

Employees can access private resources remotely.

### 5. Secure site connectivity

Different offices can communicate securely over public networks.

### 6. Cost reduction

Organizations can use the Internet rather than dedicated private physical connections for some connectivity requirements.

---

# 38. VPN Limitations

A VPN is **not a complete security solution**.

Important limitations include:

### 1. Endpoint compromise

If the user's laptop contains malware, the VPN does not magically make the laptop safe.

```text
Compromised Laptop
       │
       ▼
VPN
       │
       ▼
Company Network
```

The malware may potentially use the legitimate VPN connection.

### 2. VPN server compromise

If the VPN gateway is compromised, attackers may gain access to protected resources.

### 3. Performance overhead

Encryption and encapsulation consume processing resources.

### 4. Misconfiguration

Poor VPN configuration can create security vulnerabilities.

### 5. VPN does not equal anonymity

A VPN primarily protects traffic between the VPN endpoints. It does not make a user universally anonymous.

---

# 39. Firewall vs VPN

These two technologies are related but solve different problems.

|Feature|Firewall|VPN|
|---|---|---|
|Primary purpose|Control network traffic|Secure communication over an untrusted network|
|Main function|Allow/block traffic|Encrypt/authenticate/tunnel traffic|
|Encryption|Not necessarily|Usually|
|Access control|Yes|Yes, as part of VPN policy|
|Protects network boundary|Yes|Not primarily|
|Remote access|Can support it|Major use case|
|Traffic confidentiality|Not inherently|Yes, when encryption is used|
|Example|Block TCP port 23|Secure employee connection to company|

---

# 40. Firewall + VPN Together

In real networks, firewalls and VPNs are often used together.

```text
                       INTERNET
                           │
                           ▼
                  ┌─────────────────┐
                  │    FIREWALL     │
                  │                 │
                  │ Access Control  │
                  └───────┬─────────┘
                          │
                    VPN Gateway
                          │
                    Encrypted VPN
                          │
                          ▼
                  ┌─────────────────┐
                  │ Internal Network│
                  └─────────────────┘
```

For example:

1. Firewall permits VPN traffic.
    
2. VPN gateway authenticates the user.
    
3. VPN establishes an encrypted tunnel.
    
4. Firewall rules restrict what the VPN user can access.
    
5. Internal systems remain protected.
    

---

# 41. Firewall + Trusted System + VPN

These concepts fit together as layers.

```text
                 INTERNET
                     │
                     ▼
              ┌─────────────┐
              │  FIREWALL   │
              │             │
              │ Traffic     │
              │ Filtering   │
              └──────┬──────┘
                     │
                     ▼
               VPN Gateway
                     │
             Authentication
                     │
             Encrypted Tunnel
                     │
                     ▼
              Internal Network
                     │
                     ▼
              Trusted Systems
                     │
             ┌───────┴────────┐
             │                │
       Access Control      Auditing
             │
             ▼
         Resources
```

Each addresses a different security requirement:

- **Firewall:** controls network traffic.
    
- **VPN:** protects communication across an untrusted network.
    
- **Trusted system:** enforces security policy and controls access to resources.
    

---

# 42. Important Exam Definitions

### Firewall

> A firewall is a security mechanism that controls network traffic between trusted and untrusted networks according to a defined security policy.

### Packet-Filtering Firewall

> A firewall that filters packets based on header information such as source/destination IP address, protocol, and port number.

### Stateful Firewall

> A firewall that maintains information about active network connections and makes filtering decisions based on connection state.

### Proxy Firewall

> A firewall that acts as an intermediary between clients and servers and can inspect application-level traffic.

### Trusted System

> A system that employs security mechanisms to enforce a defined security policy and control access to protected resources.

### Trusted Computing Base

> The collection of hardware, software, and firmware components responsible for enforcing a system's security policy.

### Reference Monitor

> A security mechanism that mediates every access of a subject to an object according to the security policy.

### VPN

> A VPN is a technology that creates a secure logical tunnel over an untrusted network, typically using authentication, encryption, and integrity protection.

---

# 43. Quick Revision

```text
FIREWALL
│
├── Purpose
│   └── Control network traffic
│
├── Design Principles
│   ├── All traffic passes through firewall
│   ├── Authorization
│   ├── Secure firewall itself
│   ├── Default deny
│   ├── Least privilege
│   └── Defense in depth
│
├── Types
│   ├── Packet filtering
│   ├── Stateful inspection
│   ├── Circuit-level gateway
│   ├── Application-level gateway / proxy
│   ├── NGFW
│   ├── Host-based
│   └── Network-based
│
├── TRUSTED SYSTEMS
│   ├── Security policy
│   ├── Trusted Computing Base
│   ├── Reference Monitor
│   ├── Authentication
│   ├── Authorization
│   ├── DAC
│   └── MAC
│
└── VPN
    ├── Secure tunnel
    ├── Encryption
    ├── Authentication
    ├── Integrity
    ├── Remote-access VPN
    ├── Site-to-site VPN
    ├── Intranet VPN
    └── Extranet VPN
```

### The simplest way to remember the distinction

**Firewall → "Should this traffic be allowed?"**

**Trusted system → "Should this user/process be allowed to access this resource?"**

**VPN → "How can we securely communicate across an untrusted network?"**