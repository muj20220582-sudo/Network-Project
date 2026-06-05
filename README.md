# Network Traffic Analysis and Attack Classification

## Overview

This project analyzes multiple network traffic capture files using Wireshark.
The goal is to inspect each PCAP file, identify suspicious traffic patterns, classify the attacks, determine attackers and victims, and suggest prevention and mitigation techniques.

The project covers four traffic captures:

* `Traffic1.pcapng`
* `Traffic2.pcapng`
* `Traffic3.pcapng`
* `Traffic4.pcapng`

## Project Objectives

The main objectives of this project are to:

* Open and inspect PCAP files using Wireshark.
* Review protocol hierarchy and traffic statistics.
* Identify abnormal network behavior.
* Detect and classify attacks.
* Determine attackers, victims, and spoofed addresses.
* Perform time-series analysis for attack activity.
* Recommend prevention and mitigation controls.

## Tools Used

* **Wireshark** - packet capture analysis and filtering.
* **Wireshark I/O Graphs** - time-series traffic visualization.
* **Protocol Hierarchy Statistics** - traffic overview and protocol distribution.
* **Display Filters** - attack detection and packet filtering.

## Wireshark Filters Used

Some important filters used during the analysis:

```text
tcp.flags.syn == 1 && tcp.flags.ack == 0
icmpv6
tcp.flags.reset == 1
arp.dst.proto_ipv4 == 10.10.0.1
tls.handshake.type == 2
ip.src == 9.9.9.9 && tcp.flags.syn == 1 && tcp.flags.ack == 0
ip.src == 10.20.30.40 && tcp.flags.syn == 1 && tcp.flags.ack == 0
ip.src == 230.1.10.20 && tcp.flags.syn == 1 && tcp.flags.ack == 0
```

## Traffic 1 Analysis

### Overview

Traffic 1 contained **16,285 packets**.

Key observations:

* TCP traffic represented around **93.2%** of packets.
* HTTP traffic was very low.
* IPv4 traffic was dominant.
* ICMPv6 traffic was also observed.
* ARP traffic appeared in the capture.

### Identified Attacks

Traffic 1 included multiple suspicious activities:

1. **TCP SYN Flood Attack**
2. **ARP Spoofing / Man-in-the-Middle Attempt**
3. **ICMPv6 Flood / Reconnaissance Activity**

The SYN flood activity occurred in a short time window, around **376 to 392 seconds** into the capture.

### Attacker and Victim Information

Attacker:

```text
192.168.1.104
```

Spoofed IP addresses:

```text
192.168.1.1
192.168.1.105
```

Victims:

```text
60.254.148.81
117.18.237.29
```

## Traffic 2 Analysis

### Overview

Traffic 2 showed that **100% of the packets were TCP**, which was a strong indicator of abnormal traffic and possible DoS/DDoS behavior.

### Identified Attack

The detected attack was a:

```text
SYN Flood DDoS Attack
```

Multiple source IP addresses sent a high number of SYN packets to the same victim without completing the TCP three-way handshake.

### Attacker and Victim Information

Source IP addresses involved:

```text
10.20.30.40
230.1.10.20
9.9.9.9
```

The IP address `9.9.9.9` appeared to be used as a spoofed source IP.

Victim:

```text
172.16.112.50
```

## Traffic 3 Analysis

### Identified Attack

Traffic 3 contained one main attack:

```text
TCP SYN Port Scanning Attack
```

The attacker sent multiple SYN packets to different destination ports in sequence.

### Evidence

* 142 out of 343 packets were SYN packets without ACK.
* Destination ports were scanned sequentially from port 20 to port 90.
* The victim responded with multiple RST-ACK packets, indicating closed ports.

### Attacker and Victim Information

Attacker:

```text
135.13.216.191
```

Victim:

```text
172.16.112.50
```

## Traffic 4 Analysis

### Identified Attacks

Traffic 4 included several attacks:

1. **ARP Spoofing**
2. **TLS Interception / Man-in-the-Middle Attack**
3. **TCP Reset Flood**
4. **SYN Flood**

### Attack Description

The attacker used ARP spoofing to associate its MAC address with the gateway IP address, allowing traffic redirection.
TLS interception was then used to respond with forged TLS handshake messages and malicious certificates.
TCP Reset Flooding was used to terminate active connections by injecting forged RST packets.
SYN Flooding was also observed as a denial-of-service attempt.

### Victim Information

Victim:

```text
10.10.0.33
```

### Attacker Information

Main attacker involved in TLS spoofing:

```text
IP Address: 2.16.119.157
MAC Address: de:d7:f0:66:cf:6d
```

Secondary suspicious server:

```text
2.18.105.186
```

Spoofed gateway:

```text
10.10.0.1
```

Additional suspicious IP addresses:

```text
142.250.179.131
185.65.113.17
194.87.236.205
```

## Summary of Detected Attacks

| Traffic File | Detected Attacks                                         |
| ------------ | -------------------------------------------------------- |
| Traffic 1    | SYN Flood, ARP Spoofing / MITM, ICMPv6 Flood             |
| Traffic 2    | SYN Flood DDoS                                           |
| Traffic 3    | TCP SYN Port Scan                                        |
| Traffic 4    | ARP Spoofing, TLS Interception, TCP RST Flood, SYN Flood |

## Prevention and Mitigation

### SYN Flood

* Enable SYN cookies.
* Use TCP intercept on firewalls or routers.
* Apply rate limiting.
* Use IDS/IPS systems to detect abnormal SYN traffic.
* Block malicious or spoofed IP addresses.

### ARP Spoofing

* Enable Dynamic ARP Inspection.
* Use static ARP entries for critical systems.
* Monitor ARP table changes.
* Segment the network using VLANs.
* Use secure switch configurations.

### TLS Interception / MITM

* Use certificate pinning.
* Validate TLS certificates.
* Avoid accepting untrusted certificates.
* Use HSTS where applicable.
* Monitor for suspicious TLS handshakes.

### TCP Reset Flood

* Block unsolicited RST packets using firewall rules.
* Use IDS/IPS detection rules.
* Monitor abnormal reset packet spikes.
* Harden firewall and router filtering policies.

### Port Scanning

* Block suspicious scanning IP addresses.
* Disable unused services.
* Use firewalls to restrict exposed ports.
* Use IDS/IPS tools such as Snort or Suricata.
* Review logs for repeated failed connection attempts.

## Repository Structure

```text
Network-Traffic-Analysis/
│
├── README.md
├── Network Project.docx
└── pcaps/
    ├── Traffic1.pcapng
    ├── Traffic2.pcapng
    ├── Traffic3.pcapng
    └── Traffic4.pcapng
```

## Conclusion

This project demonstrates how Wireshark can be used to analyze network traffic and detect different types of attacks.
The analysis identified SYN flood attacks, ARP spoofing, TLS interception, TCP reset flooding, ICMPv6 suspicious activity, and TCP SYN port scanning.

The project also provides recommended mitigation techniques to reduce the risk of these attacks and improve network security.

## Disclaimer

This project is for educational and defensive cybersecurity purposes only.
All traffic analysis should be performed in an authorized lab environment or on traffic captures that you are allowed to inspect.
::: 
