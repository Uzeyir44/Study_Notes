
## 1.2 Network Components

### 1.2.1 Host Roles

**Concept:** Any device that sends and receives messages on a network is called a **host** (or end device). In a TCP/IP network, a host is defined by having an IP address, which allows it to be uniquely identified.

**The Client-Server Model:**

- **Problem:** Data and services need to be structured so that multiple devices can reliably request and access shared resources without localized bottlenecks or data inconsistency.
    
- **Solution:** Divide roles logically.
    
- **Server:** A host equipped with software that allows it to provide information (like web pages, email, or files) to other network devices. Servers typically have dedicated hardware, static IP addresses, and high availability.
    
- **Client:** A host that has software for requesting and displaying the information obtained from a server.
    
- **Dual Roles:** A single physical machine can run multiple software processes. A computer can run a web server process (acting as a server) while simultaneously running a web browser (acting as a client to a different server).
    

### 1.2.2 Peer-to-Peer

**Concept:** Peer-to-Peer (P2P) networking is a distributed application architecture where hosts act as both clients and servers simultaneously.

**Why it exists:** To decentralize resource sharing without needing dedicated server infrastructure. Devices communicate directly, requesting files from peers and simultaneously serving files to other peers.

**Client-Server vs. Peer-to-Peer**

|**Feature**|**Client-Server**|**Peer-to-Peer (P2P)**|
|---|---|---|
|**Architecture**|Centralized|Decentralized|
|**Role Distribution**|Strict (Dedicated servers and clients)|Fluid (Hosts act as both simultaneously)|
|**Scalability**|High (Easily scales with enterprise networks)|Low (Performance degrades as network grows)|
|**Security & Administration**|Centralized (Easy to enforce policies and backup)|Decentralized (Each user manages their own security)|
|**Cost & Complexity**|Higher cost (Dedicated hardware/OS), higher complexity|Lower cost, simple to set up|
|**Use Cases**|Corporate file sharing, Web hosting, Enterprise databases|Home networks, BitTorrent, simple file transfers|

**Limitations:** P2P becomes highly impractical in growing networks because there is no centralized authentication, backups are localized and unmanaged, and the performance of individual workstations degrades when multiple peers request their resources.

### 1.2.3 End Devices

**Concept:** An **end device** is the source or destination of a message transmitted over the network.

**Relationship with Hosts:** The terms "host" and "end device" are often used interchangeably, but "end device" emphasizes the hardware's position at the edge of the network infrastructure, interacting with the user or physical environment, whereas "host" emphasizes its logical participation via an IP address.

**Examples:** Computers (workstations, laptops, servers), network printers, VoIP phones, security cameras, and IoT sensors.

**How it works:** To distinguish one end device from another, each device is assigned an address. When an end device initiates communication, it uses the address of the destination end device to specify where the message should be delivered.

### 1.2.4 Intermediary Devices

**Concept:** Intermediary devices interconnect end devices or connect multiple individual networks to form an internetwork.

**Problem:** A network cannot simply connect every end device directly to every other end device (a full-mesh topology) because the amount of cabling and ports required would be physically and economically impossible to scale.

**Solution:** Intermediary devices act as aggregation and routing nodes. They do not originate or terminate the primary data payloads; they direct them.

**Functions:**

- Regenerate and retransmit data signals.
    
- Maintain information about what pathways exist through the network.
    
- Notify other devices of errors and communication failures.
    
- Direct data along alternate pathways when there is a link failure.
    
- Classify and direct messages according to QoS priorities.
    
- Permit or deny the flow of data based on security settings.
    

**Examples:**

- **Switches:** Connect end devices on a single local network.
    
- **Routers:** Connect distinct networks together and determine the best path for data.
    
- **Wireless Access Points (WAPs):** Provide wireless connectivity to end devices within a local network.
    
- **Firewalls:** Analyze data traffic to enforce security policies.
    

### 1.2.5 Network Media

**Concept:** Communication across a network is carried on a medium. The medium provides the channel over which the message travels from source to destination.

**Types of Media:**

1. **Metal Wires within Cables (Copper):** Data is encoded into electrical impulses.
    
    - _Characteristics:_ Inexpensive, easy to install, but susceptible to electromagnetic interference (EMI) and distance limitations.
        
2. **Glass or Plastic Fibers (Fiber-optic):** Data is encoded as pulses of light.
    
    - _Characteristics:_ Expensive, harder to install, but can carry massive bandwidth over long distances and is immune to EMI.
        
3. **Wireless Transmission:** Data is encoded via modulation of specific frequencies of electromagnetic waves.
    
    - _Characteristics:_ Highly mobile and flexible, but susceptible to physical obstructions, interference, and security interception.
        

### 1.2.6 Check Your Understanding — Network Components

1. What distinguishes a host acting as a server from a host acting as a client?
    
2. Why is a peer-to-peer network inappropriate for a corporate environment with hundreds of users?
    
3. If an end device is the source or destination of data, what is the fundamental purpose of an intermediary device?
    
4. What physical limitation of copper wiring might force a network engineer to choose fiber-optic cabling for a specific link?
    

## 1.3 Network Representations

### 1.3.1 Network Representations

**Concept:** Networks are complex physical and logical systems. Network professionals use visual representations (diagrams) to document, troubleshoot, and design them.

Network diagrams use specific symbols to represent end devices, intermediary devices, and network media. Understanding these universally accepted symbols is a prerequisite for reading network architecture documentation.

### 1.3.2 Topology Diagrams

**Concept:** A topology is the structural arrangement of a network.

**Problem:** Physical wiring does not always dictate how data actually flows. A technician needs to know where a cable physically runs, but a network engineer needs to know which IP subnets exist and how traffic routes between them.

**Solution:** Networks are documented using two distinct types of topology diagrams.

- **Physical Topology:** Illustrates the physical location of intermediary devices, port installations, and cable routing. It answers: _Where is the hardware actually located? Which physical port is used?_
    
- **Logical Topology:** Illustrates devices, ports, and the IP addressing scheme. It answers: _How does data flow through the network? What are the subnets and logical boundaries?_
    

**Relationship:** A network might have a physical star topology (all cables run to one central switch in a closet) but function logically as a single contiguous subnet. Understanding the abstraction between physical infrastructure and logical addressing is fundamental to modern networking.

### 1.3.3 Check Your Understanding — Network Representations and Topologies

1. Why does a network engineer need both a physical and a logical topology diagram?
    
2. If you are assigning IP addresses to servers, which topology diagram are you primarily referencing?
    
3. If you are dispatching a technician to replace a broken switch, which topology diagram are you primarily referencing?
    

## 1.4 Common Types of Networks

### 1.4.1 Networks of Many Sizes

Networks range from simple two-computer connections to complex global infrastructures.

- **Small Home Networks:** Connect a few computers to each other and the Internet.
    
- **Small Office/Home Office (SOHO):** Enable computers within a home or remote office to connect to a corporate network or access centralized resources.
    
- **Medium to Large Networks:** Connect hundreds or thousands of computers globally (enterprises, universities).
    
- **World Wide Networks:** The Internet is a network of networks connecting hundreds of millions of computers worldwide.
    

### 1.4.2 LANs and WANs

**Concept:** Networks are primarily categorized by their geographic span and the entity that manages them.

**Local Area Network (LAN):** A network infrastructure that provides access to users and end devices in a small geographical area.

- Typically owned, managed, and controlled by a single individual or organization.
    
- Provides high-speed bandwidth to internal end devices and intermediary devices.
    

**Wide Area Network (WAN):** A network infrastructure that provides access to other networks over a wide geographical area.

- Typically managed by a Telecommunications Service Provider (TSP) or Internet Service Provider (ISP).
    
- Interconnects LANs over long distances.
    
- Typically provides slower speed links between LANs compared to internal LAN speeds.
    

**LAN vs. WAN**

|**Characteristic**|**LAN**|**WAN**|
|---|---|---|
|**Geographic Scope**|Small (a home, office building, or campus)|Large (cities, countries, global)|
|**Ownership**|Single organization or individual|Service providers (ISPs/TSPs)|
|**Bandwidth/Speed**|Extremely high (1 Gbps, 10 Gbps, up to 100 Gbps)|Slower, limited by cost of leased lines|
|**Connection Example**|Connecting PCs to a local switch|Connecting a branch office router to HQ via an ISP|

### 1.4.3 The Internet

**Concept:** The Internet is a worldwide collection of interconnected networks (a "network of networks").

**How it works:** There is no single central authority that runs the Internet. Instead, it relies on shared, agreed-upon standards (protocols) developed by organizations like the IETF and IEEE. LANs connect to WANs, and WANs connect to each other through ISPs. ISPs themselves connect at higher levels to tier-1 network providers, forming a massive, redundant global web of intermediary devices routing traffic based on IP addresses.

### 1.4.4 Intranets and Extranets

**Concept:** Organizations need to control access to their data based on the user's relationship with the organization.

- **Intranet:** A private connection of LANs and WANs that belongs to an organization. It is accessible only to the organization's members, employees, or others with authorization. (e.g., An internal corporate HR portal).
    
- **Extranet:** A network that provides safe and secure access to individuals who work for a different organization but require access to the organization's data. (e.g., A portal for suppliers to check inventory, or a hospital system providing access to external doctors).
    
- **Internet:** Global, public access.
    

### 1.4.5 Check Your Understanding — Common Types of Networks

1. Why do WANs generally have lower bandwidth than LANs?
    
2. If a company wants to share real-time inventory data with a trusted external supplier, should they use an intranet, extranet, or the open Internet?
    
3. What does the phrase "network of networks" imply about the Internet's architecture?
    

## 1.5 Internet Connections

### 1.5.1 Internet Access Technologies

Users and organizations require a physical and logical connection to an ISP to access the Internet. The technology chosen depends on availability, cost, and required bandwidth.

### 1.5.2 Home and Small Office Internet Connections

Home and SOHO environments typically require asymmetric connections (higher download speeds than upload speeds) and share infrastructure with neighbors.

- **Cable:** Offered by television service providers. The internet signal rides on the same coaxial cable that delivers cable TV. It provides high bandwidth, high availability, and an always-on connection.
    
- **DSL (Digital Subscriber Line):** Provides high bandwidth, always-on connection over traditional copper telephone lines. Requires a special modem to separate the DSL signal from the telephone voice signal.
    
- **Cellular:** Uses a mobile phone network to connect. Performance depends on the phone and the cell tower's technology (4G/5G).
    
- **Satellite:** Good for rural areas lacking wired infrastructure. Suffers from high latency due to the massive physical distance signals must travel to orbit and back.
    
- **Dial-up Telephone:** An outdated, very low bandwidth option using a modem to dial a phone number for connection.
    

### 1.5.3 Business Internet Connections

Corporate connections demand higher bandwidth, symmetric speeds (equal upload/download), strict service level agreements (SLAs), and dedicated hardware.

- **Dedicated Leased Line:** Reserved circuits within the service provider's network that connect geographically separated offices for private voice and/or data networking. High cost but guaranteed performance.
    
- **Metro Ethernet:** Extends LAN access technology (Ethernet) into the WAN, providing high-bandwidth business connectivity.
    
- **Business DSL:** Symmetric Digital Subscriber Lines (SDSL) provide equal upload and download speeds, unlike asymmetric home DSL.
    
- **Satellite:** Used when a business location is completely isolated from terrestrial infrastructure.
    

### 1.5.4 The Converging Network

**Concept:** Historically, separate networks were built for distinct purposes (a network for computer data, a separate network for telephones, a separate network for television broadcasting).

**Convergence:** A converged network multiplexes voice, video, and data over a single, shared infrastructure using the same set of rules, agreements, and standards (TCP/IP).

**Why it is useful:** It massively reduces physical infrastructure costs and administrative overhead. Instead of wiring a building with phone cables, coax cables, and ethernet cables, a business wires the building with ethernet, and routers/switches handle the separation and prioritization of different data types logically.

### 1.5.5 Packet Tracer — Network Representation

_Note: Packet Tracer is Cisco’s network simulation tool used extensively in the CCNA._

This introductory activity is designed to teach you how to navigate a simulated network environment.

- **What you learn:** How to interact with the GUI, identify intermediary vs. end devices by their icons, and toggle between logical and physical workspaces.
    
- **Key takeaway:** Packet Tracer builds the fundamental mental map you will use to configure and troubleshoot simulated CLI environments in future modules.
    

## 1.6 Reliable Networks

### 1.6.1 Network Architecture

Networks must support current operations and accommodate future growth. The design principles that ensure networks remain reliable are called network architecture. A reliable network architecture rests on four fundamental traits: Fault Tolerance, Scalability, Quality of Service (QoS), and Security.

### 1.6.2 Fault Tolerance

**Concept:** The ability of a network to continue functioning normally even if a component (link, router, or switch) fails.

- **How it works:** It is achieved through **redundancy**—having multiple paths to a destination. If one router goes down, routing protocols dynamically recalculate the path and send traffic through an alternate router.
    
- **Example:** If a backhoe cuts a fiber-optic cable leading to a company's primary ISP, the network automatically fails over to a secondary backup ISP without users losing connection.
    

### 1.6.3 Scalability

**Concept:** The ability to accept new products, applications, and users without impacting the performance of existing users.

- **How it works:** Scalability is achieved by adopting a hierarchical, modular design. Instead of plugging everything into one massive switch (which creates a bottleneck), networks are broken into layers (access, distribution, core). You can add new subnets or branches easily without redesigning the whole network.
    

### 1.6.4 Quality of Service (QoS)

**Concept:** The mechanism to manage network resources to guarantee a certain level of performance to specific types of data.

- **Problem:** As networks converge, time-sensitive traffic (like a live VoIP phone call) travels on the same wire as non-time-sensitive traffic (like an email download). If the network is congested, packets are delayed or dropped.
    
- **Why it matters:** Dropping an email packet for 2 seconds is unnoticeable (it will just be retransmitted). Delaying a voice packet for 2 seconds results in a robotic, broken, and unusable phone call.
    
- **Solution:** QoS allows routers to prioritize voice/video traffic over web/data traffic, ensuring low latency and low jitter for real-time communications even during network congestion.
    

### 1.6.5 Network Security

**Concept:** Protecting the physical infrastructure and the data flowing through it.

Security is universally modeled on the **CIA Triad**:

1. **Confidentiality:** Data is only readable by authorized users (achieved via encryption and authentication).
    
2. **Integrity:** Data has not been altered in transit (achieved via hashing and digital signatures).
    
3. **Availability:** The network and data are accessible when needed (achieved via redundancy, backups, and preventing Denial of Service attacks).
    

Security must address both physical threats (someone stealing a server) and logical threats (malware, hackers).

### 1.6.6 Check Your Understanding — Reliable Networks

1. Why is redundancy the primary mechanism for fault tolerance?
    
2. If an employee is complaining that their VoIP phone calls are choppy when large file transfers occur on the network, which network architecture trait is failing?
    
3. How does a hierarchical network design relate to scalability?
    

## 1.7 Network Trends

### 1.7.1 Recent Trends

Networking is shifting from a static, desktop-bound paradigm to a highly mobile, distributed, and integrated paradigm. The driving forces are the miniaturization of computing, the ubiquity of high-bandwidth wireless, and the shift of compute resources to the cloud.

### 1.7.2 Bring Your Own Device (BYOD)

**Concept:** The policy of allowing employees to use their personal devices (laptops, smartphones, tablets) to access enterprise networks and corporate data.

- **Benefits:** Cost savings on hardware, employee flexibility, and convenience.
    
- **Security Challenge:** The network administrator does not own or control the endpoint. If an employee downloads malware at home, they can bring it straight into the corporate network. It requires robust network-level access controls to mitigate.
    

### 1.7.3 Online Collaboration

Networks enable teams to work together globally in real time. Tools like Cisco Webex, Microsoft Teams, and Slack rely on robust network infrastructure to synchronize chat, files, and live presence globally with minimal latency.

### 1.7.4 Video Communications

Video is becoming the default standard for collaboration.

- **Networking impact:** Video places immense strain on network infrastructure. It requires high continuous bandwidth and strict QoS to prevent buffering and audio/video desynchronization.
    

### 1.7.5 Video — Cisco Webex for Huddles

_Context from course material: Demonstrating collaboration tools._

The technical takeaway is that localized physical spaces (huddle rooms) are now endpoints on the global network, integrating local hardware (cameras/screens) with cloud infrastructure to seamlessly bridge physical and logical environments.

### 1.7.6 Cloud Computing

**Concept:** The delivery of computing services (servers, storage, databases, networking, software) over the Internet ("the cloud").

- **Why it exists:** Organizations no longer want the capital expense of buying, powering, and maintaining physical servers. They rent compute/storage on a pay-as-you-go basis.
    
- **Types:** Public (AWS/Azure), Private (internally hosted virtualized data centers), Hybrid (mixing both), and Custom clouds.
    
- **Networking relationship:** Cloud computing is fundamentally impossible without reliable networking. The network _is_ the bus connecting the CPU (the cloud) to the monitor (the user).
    

### 1.7.7 Technology Trends in the Home

The integration of networking into everyday objects is called the **Internet of Things (IoT)**.

Ovens, lights, security systems, and thermostats are now equipped with wireless NICs (Network Interface Cards) and connect to the home LAN, routing data to cloud servers to allow users to control physical home environments via smartphone apps.

### 1.7.8 Powerline Networking

**Concept:** Using existing electrical wiring in a home or building to transmit network data.

- **How it works:** A device is plugged into a standard electrical outlet and connects to the router. Another device is plugged into another outlet elsewhere in the building. Data is modulated at a high frequency over the AC power wires.
    
- **Use case:** Useful when Wi-Fi signals cannot penetrate thick walls and running new Ethernet cable is too expensive or impossible.
    

### 1.7.9 Wireless Broadband

**Concept:** Providing high-speed Internet access via wireless signals over large areas.

- **Wireless Internet Service Provider (WISP):** An ISP that connects subscribers to a designated access point or hotspot using similar wireless technologies found in home Wi-Fi, but with higher-power transmitters. Primarily used in rural environments where running cable/fiber is cost-prohibitive.
    

### 1.7.10 Check Your Understanding — Network Trends

1. Why does BYOD present a massive security challenge to a corporate network?
    
2. How does cloud computing change an organization's reliance on their WAN connection?
    
3. In what scenario would Powerline networking be superior to Wi-Fi?
    

## 1.8 Network Security

### 1.8.1 Security Threats

Network security is an integral part of networking; a compromised network is an unavailable network.

**Common External Threats:**

- **Viruses, Worms, and Trojan Horses:** Malicious software that can infect systems, destroy data, and self-propagate across LANs.
    
- **Spyware and Adware:** Software secretly installed to track user behavior or serve unwanted ads.
    
- **Zero-day Attacks:** Exploiting software vulnerabilities before the software vendor even knows they exist or has released a patch.
    
- **Threat Actor Attacks:** Malicious individuals actively trying to compromise a network (hacking).
    
- **Denial of Service (DoS):** Overwhelming a network device (like a server or router) with bogus traffic so it cannot respond to legitimate traffic.
    
- **Data Interception and Theft:** Capturing private data as it traverses unencrypted network links.
    

**Internal Threats:** Disgruntled employees, lost or stolen hardware, or simply careless users writing down passwords.

### 1.8.2 Security Solutions

A single security tool cannot solve all problems. Networks require a defense-in-depth approach.

- **Antivirus and Antispyware:** Installed on end devices to protect the host itself.
    
- **Firewall Filtering:** Installed as intermediary devices to block unauthorized access to the network based on IP addresses and port numbers.
    
- **Dedicated Firewall Systems:** Used in enterprise networks to provide advanced application-layer inspection.
    
- **Access Control Lists (ACLs):** Configured on routers to filter traffic based on security policies.
    
- **Intrusion Prevention Systems (IPS):** Analyze traffic in real-time to detect and stop zero-day attacks or complex malware.
    
- **Virtual Private Networks (VPNs):** Provide secure, encrypted tunnels for remote workers accessing the corporate network over the public Internet.
    

### 1.8.3 Check Your Understanding — Network Security

1. What is the fundamental difference between a virus/worm and a DoS attack?
    
2. Why is a firewall an intermediary device rather than an end device?
    
3. How does a VPN address the threat of data interception on the public Internet?
    

## Final Self-Test

1. What is the primary difference between a host and an intermediary device?
    
2. True or False: In a peer-to-peer network, devices can simultaneously act as both a client and a server.
    
3. Which network component is responsible for regenerating and retransmitting data signals?
    
4. What type of topology diagram shows the IP addressing scheme and how data flows?
    
5. A company needs to securely share specific shipment tracking data with a partner logistics company. Should they use an Intranet, Extranet, or Internet?
    
6. Which type of Internet connection is characterized by high bandwidth over shared television coaxial cables?
    
7. What does the term "network convergence" mean?
    
8. A router has dual power supplies and dual links to different ISPs. What network architecture characteristic is this demonstrating?
    
9. Why is Quality of Service (QoS) critical for Voice over IP (VoIP) traffic?
    
10. Which characteristic of the CIA triad ensures that a malicious actor cannot intercept and read a password sent over the network?
    
11. What is the main security risk associated with BYOD policies?
    
12. A rural customer cannot get cable or DSL internet and cannot afford satellite latency. What technology uses localized high-power transmitters to provide internet access?
    
13. What type of attack aims to make a service unavailable by overwhelming it with traffic?
    
14. Which security device is primarily responsible for blocking unauthorized access to a network at the perimeter?
    
15. A remote worker needs to access the corporate Intranet securely from a public coffee shop Wi-Fi. What technology should they use?
    

## Answers

1. A host (end device) is the source or destination of network traffic. An intermediary device connects hosts and directs/routes the traffic between them.
    
2. True. P2P architectures lack dedicated servers; devices fulfill both roles.
    
3. Intermediary devices (specifically switches, routers, and repeaters).
    
4. Logical topology diagram.
    
5. Extranet. It is private data shared securely with a trusted external entity.
    
6. Cable internet.
    
7. Multiplexing voice, video, and data over a single, unified physical and logical IP infrastructure.
    
8. Fault tolerance (specifically, redundancy).
    
9. Voice traffic is highly sensitive to delay and jitter. Without QoS prioritization, voice packets might drop during congestion, degrading the call.
    
10. Confidentiality (typically achieved via encryption).
    
11. The organization does not have administrative control over the personal devices, meaning they might bring malware or compromised software directly into the internal network.
    
12. Wireless Internet Service Provider (WISP) / Wireless Broadband.
    
13. Denial of Service (DoS) attack.
    
14. Firewall.
    
15. Virtual Private Network (VPN).
    

## Final Review Sections

### Key Terms

- **Host / End Device:** The source or destination of a message.
    
- **Intermediary Device:** Hardware that connects end devices and forwards traffic.
    
- **Physical/Logical Topology:** The hardware layout vs. the IP/data flow layout.
    
- **LAN / WAN:** Local Area Network (local control, fast) vs. Wide Area Network (ISP controlled, interconnects LANs).
    
- **Convergence:** One network carrying voice, video, and data.
    
- **Fault Tolerance:** Network resilience through redundancy.
    
- **Scalability:** Network growth without performance degradation via modular design.
    
- **QoS (Quality of Service):** Prioritizing time-sensitive traffic.
    

### Common Confusions

- **Host vs. End Device:** Technically identical in most contexts. "End device" highlights the physical edge of the network; "host" highlights logical addressing/participation.
    
- **Physical vs. Logical Topology:** Physical = Cables and hardware ports. Logical = Subnets, IP addresses, and routing paths.
    
- **Intranet vs. Extranet:** Intranet = Employees only. Extranet = Employees + Trusted Partners/Suppliers.
    
- **Bandwidth vs. Latency:** Bandwidth is how much data fits in the pipe (throughput). Latency is how long the data takes to travel from end to end (delay).
    

### Must Understand (First Principles)

- **The necessity of Intermediary Devices:** Why we can't just wire every PC directly to every other PC. Intermediary devices solve the physical routing scalability problem.
    
- **The Client-Server logic:** Why separating resource providers (servers) from resource consumers (clients) allows networks to scale effectively.
    
- **Network Architecture Pillars:** How Fault Tolerance, Scalability, QoS, and Security function together to make a network usable for a business.
    

### Must Remember

- The primary symbols used for Routers, Switches, Firewalls, and End Devices.
    
- The CIA Triad for security (Confidentiality, Integrity, Availability).
    
- The difference between LAN and WAN ownership and speeds.
    

### Why This Matters Later

- **End Devices & Media** $\rightarrow$ These are the physical and data link layers (OSI Layers 1 & 2), preparing you for Ethernet frames and MAC addresses.
    
- **LAN/WAN & Logical Topologies** $\rightarrow$ This is the foundation of IP addressing and subnetting. You will soon configure routers to logically divide LANs and route them across WANs.
    
- **Client/Server** $\rightarrow$ Prepares you for TCP/UDP and the Application Layer (DNS, HTTP, DHCP).
    
- **Network Security** $\rightarrow$ Prepares you for configuring Access Control Lists (ACLs) on Cisco routers to explicitly permit or deny traffic flows.
    
- **IoT & Convergence** $\rightarrow$ As a CS/Embedded Systems student, this introduces how localized microcontrollers (IoT) leverage IP networks to interact with global cloud infrastructure, which will be highly relevant in ML/Robotics telemetry.