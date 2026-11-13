# Research: Linux Networking

## 1. Introduction

This are the group of tooling, protocol and configuration of the linux kernel, allow to an SO to keep in touch with another through LAN or Internet

## 2. Core Concepts

Core concepts of networking are IP Addres, route tables, dns, ports and trafic control.

##### - IP -> Internet procotol, set of rule and standands for data packet travel through devices

##### - IP Address -> Unique numerical label assigned to a device in a network, it have two versions IPv4 (32bit decimal notation) and IPv6(128 bit hexadecimal notation)

##### - Route Tables -> Data table stored in a host that list the path to particular network destination, contain the information on how data packet go through interconected networks to reach their final destiny

##### - DNS -> Domain Name System, its like a phonebook in the internet, translate readable string to numerical IP Addresses

##### - Port -> Its an endpoint for software based communication, its a number of 16 bit from 0 to 65535, this tool opens operating systems to direct network traffic

#### Trafic Control -> Its the supervision, control and managment of the traffic that passed in the server

#### Sockets -> Its an abstraction concept, a set of information of the device host for entablish comunication betweeen two or more devices. Socket identify IP, PORT and PROTOCOL for the communication. It have two protocols TCP (SOCK_STREM) and UDP (SOCK_DGRAM), basicaly with and without connection. We can think Socket as a communication endpoint where the device could communication between an app/device and change data with and interface

## 3. How It Works

Linux relies on TCP/IP model for manage networking communication based on Kernel and User space

Kernel Space is the manament of the network protocols, send and listening
User space just execute the apps that uses networking like HTTP, SSH, etc.

A basic Step By Step:
1. Application requests a socket.
2. Kernel creates and manages the socket.
3. Application sends data through the socket.
4. Kernel processes and transmits the network packets.
5. Destination kernel receives and processes the packets.
6. Destination application reads the received data through its socket.

Kernel have three fundamental structures

| NAME | DESCRIPTION |
| - | - |
| struck socket | General representation of socket |
| struct sock | Internal representation of the socket, store protocol information and connection status |
| struc sk_buff | network data packet representation and information |

Socket -> Communication point
Sock -> Internal status of the communication
sk_buff -> Data packet

## 4. Practical Applications

We can applied on the server infraestructure using protocols, credentiuals, users, connections, etc.
Also we can seeing linux total potentail in web servers, cloud enviroments, kernel features, command lines utilities on remote administration (SSH), firewalls development, security on OS Kernel, troubleshooting

## 5. Advantages and Limitations

Linux networking had advantages

- Cost: as a open source nature this aspect make it low cost-effective solution. A global community of developers are actively monitoring linux networking and troubleshooting issues as they arise
- Security: As an open source OS you can configure the security capabilities.
- Reliability: Its considred high reliable when complex workloads
- Flexibility: its highly customizable, giving users the permissions to infrastructure all the requirements for fit their needs
- Support: The Linux OS and Linux networking support are community-based and free

But also have limitations
For example the learning curve is high, you need to understand an learn new concepts and get used terminal this aspect may not be approachable for some users
Also there is a software compatibility, most of software targets to windows or macos first, simply because thats where the desktop basic users are windows based

## 6. Comparison and Alternatives

#### A. Native Linux Networking vs Docker Networking

##### Native Linux Networking: Uses the Linux kernel networking stack to manage network interfaces, IP addresses, routing tables, ports and firewall rules.

##### Docker Networking: Uses Linux networking capabilities to create isolated virtual networks and enable communication between containers.  Docker provides different networking drivers, including bridge, host and overlay.

#### B. Network Management Tools

Linux provides different tools and services for managing network configurations.

| Tool | Description |
| - | - |
| NetworkManager | High-level network management service, commonly used on desktop environments but also available on servers. |
| systemd-networkd | Lightweight network management service used to configure and manage network interfaces. |
| iproute2 | Collection of command-line utilities used to inspect and modify network interfaces, addresses, routes and other kernel networking configurations. |
| Netplan | Network configuration utility commonly used in Ubuntu, which applies configurations through backends such as NetworkManager or systemd-networkd.|

#### C. Physical vs Virtual Networking

Physical Networking: Uses physical network interfaces and hardware devices to transmit data.

Virtual Networking: Uses software-based interfaces, bridges and virtual networks to enable communication between virtual environments.

Cloud Networking: Uses virtual networking infrastructure provided by cloud platforms, including subnets, routing tables and security groups.

## 7. Application in ForgeOps

ForgeOps will rely on linux networking and command utilities for a better security and flexibility on proccess lifecycle

## 8. Personal Conclusions

What did you learn?

With this concept i learnt about networking, kernel proccesses and communication between them, how to manage linux server with their tools.

## 9. References

- Official documentation
- Technical articles
- Books or other reliable sources

[IBM LINUX NETWORKING](https://www.ibm.com/think/topics/linux-networking)
[Linux basics](https://dev.to/sreekanth_kuruba_91721e5d/linux-networking-basics-for-beginners-3e2e)
[Scaler](https://www.scaler.com/topics/advantage-and-disadvantage-of-linux/)
[Linux Networking](https://linux-kernel-labs.github.io/refs/heads/master/labs/networking.html)