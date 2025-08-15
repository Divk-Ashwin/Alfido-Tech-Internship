# Network Reconnaissance & Scanning with Nmap

> **Task: Reconnaissance & Scanning**  
> A comprehensive study of network enumeration using Nmap against authorized public targets.

[![Nmap Version](https://img.shields.io/badge/Nmap-7.97-blue)](https://nmap.org/)

---

## 📋 Objectives

- **Host Discovery**: Identify live targets and network reachability
- **Port Enumeration**: Discover open TCP ports and services  
- **Service Detection**: Identify service versions and banners
- **OS Fingerprinting**: Attempt operating system detection
- **Network Mapping**: Use traceroute to understand network topology
- **Script Scanning**: Execute NSE scripts for additional reconnaissance

---

## 🎯 Scope & Environment

| Parameter | Value |
|-----------|-------|
| **Scan Date** | 2025-08-08 |
| **Tool Version** | Nmap 7.97 |
| **Primary Target** | scanme.nmap.org |
| **Secondary Target** | example.com |
| **Platform** | Windows Command Prompt |

⚠️ **Authorization Note**: Only scan systems with explicit permission. These targets are publicly authorized for testing.

---

## 📁 Repository Structure

```
reconnaissance-scanning/
├── reports/
│   ├── 2025-08-08_nmap_scanme_basic.txt
│   ├── 2025-08-08_nmap_scanme_example_dual.txt
│   ├── 2025-08-08_nmap_scanme_ping.txt
│   ├── 2025-08-08_nmap_scanme_selected_ports.txt
│   ├── 2025-08-08_nmap_scanme_fast.txt
│   ├── 2025-08-08_nmap_scanme_service_version.txt
│   ├── 2025-08-08_nmap_scanme_os_detect.txt
│   ├── 2025-08-08_nmap_scanme_aggressive.txt
│   ├── 2025-08-08_nmap_scanme_default_scripts.txt
│   ├── 2025-08-08_nmap_scanme_ssl_heartbleed.txt
│   ├── 2025-08-08_nmap_scanme_output_to_file.txt
│   ├── 2025-08-08_nmap_scanme_no_dns.txt
│   ├── 2025-08-08_nmap_scanme_traceroute.txt
│   └── 2025-08-08_nmap_scanme_T4.txt
├── README.md
├── .gitignore
```

## What I Learned
- Started with fast scans (-sn, -F) to map hosts/ports, then used deeper scans (-sV, -A) for details.
- Read port states accurately: open vs closed vs filtered vs tcpwrapped, and chose next steps accordingly.
- Recognized limits of version detection; validated banners with multiple probes when needed.
- Treated OS detection (-O/-A) as heuristic due to hop distance and filtering; avoided overconfidence.
- Used traceroute to understand network path/latency and identify CDN/backbone hops.
- Kept outputs organized (-oN + clear filenames in reports/) for transparency and reproducibility.
- Followed ethical scope: scanned only approved targets and used safe NSE scripts.


---

## 🛠️ Commands and Results

### 1. Basic TCP Scan

**Purpose**: Default scan of top 1,000 common ports

```bash
nmap scanme.nmap.org
```

**Key Results**:
```
Open Ports: 21/ftp, 22/ssh, 80/http, 554/rtsp, 1723/pptp, 9929/nping-echo, 31337/Elite
Filtered: 25/smtp, 135/msrpc, 139/netbios-ssn, 179/bgp, 445/microsoft-ds, 646/ldp
Host Status: Up (0.28s latency)
```

---

### 2. Multiple Target Scan

**Purpose**: Scan two hosts simultaneously

```bash
nmap scanme.nmap.org example.com
```

**Key Results**:
- **scanme.nmap.org**: Same results as basic scan
- **example.com**: 21/ftp, 80/http, 443/https, 554/rtsp, 1723/pptp (Akamai CDN detected)

---

### 3. Host Discovery (Ping Scan)

**Purpose**: Quick host reachability check

```bash
nmap -sn scanme.nmap.org
```

**Key Results**:
```
Host Status: Up (0.27s latency)
Scan Time: 0.43 seconds
```

---

### 4. Selected Ports Scan

**Purpose**: Target specific ports of interest

```bash
nmap -p 22,80,443 scanme.nmap.org
```

**Key Results**:
```
22/tcp   open    ssh
80/tcp   open    http  
443/tcp  closed  https
```

---

### 5. Fast Scan

**Purpose**: Quick scan of top 100 ports

```bash
nmap -F scanme.nmap.org
```

**Key Results**:
```
Open: 21, 22, 80, 554, 1723
Filtered: 25, 135, 139, 179, 445, 646
Closed: 89 ports (not shown)
```

---

### 6. Service & Version Detection

**Purpose**: Identify services and their versions

```bash
nmap -sV scanme.nmap.org
```

**Key Results**:
```
22/tcp   ssh          OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.13
80/tcp   http         Apache httpd 2.4.7 (Ubuntu)
9929/tcp nping-echo   Nping echo
21/tcp   tcpwrapped   (service detection blocked)
Service Info: OS: Linux
```

---

### 7. OS Detection

**Purpose**: Attempt to identify target operating system

```bash
nmap -O scanme.nmap.org
```

**Key Results**:
```
Device Type: general purpose | load balancer | terminal
OS Guesses: 
- FreeBSD 6.2-RELEASE (90%)
- F5 BIG-IP Edge Gateway (85%)
- Wyse P class PCoIP client (85%)
Network Distance: 25 hops
```

**Note**: No exact OS match due to network distance and filtering

---

### 8. Aggressive Scan

**Purpose**: Comprehensive scan with OS detection, version detection, script scanning, and traceroute

```bash
nmap -A scanme.nmap.org
```

**Key Results**:
```
SSH Host Keys Detected:
- DSA: ac:00:a0:1a:82:ff:cc:55:99:dc:67:2b:34:97:6b:75
- RSA: 20:3d:2d:44:62:2a:b0:5a:9d:b5:b3:05:14:c2:a6:b2
- ECDSA: 96:02:bb:5e:57:54:1c:4e:45:2f:56:4c:4a:24:b2:57
- ED25519: 33:fa:91:0f:e0:e1:7b:1f:6d:05:a2:b0:f1:54:41:56

HTTP Service:
- Title: "Go ahead and ScanMe!"
- Server: Apache/2.4.7 (Ubuntu)
- Favicon: Nmap Project

OS: Linux 4.19-5.15 (93% confidence)
Traceroute: 23 hops via NTT/Akamai network
```

---

### 9. Default NSE Scripts

**Purpose**: Run default Nmap scripts for additional enumeration

```bash
nmap --script=default scanme.nmap.org
```

**Key Results**:
- SSH host keys enumerated
- HTTP title and favicon detected
- Standard service banners collected

---

### 10. Targeted Vulnerability Script

**Purpose**: Test for specific vulnerabilities (Heartbleed)

```bash
nmap --script=ssl-heartbleed scanme.nmap.org
```

**Key Results**:
- No SSL/TLS services detected on standard ports
- No Heartbleed vulnerability found

---

### 11. Output to File

**Purpose**: Demonstrate output redirection

```bash
nmap -oN output.txt scanme.nmap.org
```

**Key Results**: Same as basic scan, saved to output.txt

---

### 12. No DNS Resolution

**Purpose**: Skip reverse DNS lookups for speed

```bash
nmap -n scanme.nmap.org
```

**Key Results**: Same port results, faster execution (7.38s vs ~8s)

---

### 13. Traceroute Analysis

**Purpose**: Map network path to target

```bash
nmap --traceroute scanme.nmap.org
```

**Key Network Path**:
```
1    2ms    reliance.reliance (192.168.29.1)          [Local Gateway]
2-7  4-8ms  10.6.144.1 → Private ISP Infrastructure
11   214ms  103.198.140.205                           [ISP Transit]
12   241ms  ae-21.a02.nycmny17.us.bb.gin.ntt.net    [NTT Backbone]
13   263ms  ae-1.akamai.nycmny17.us.bb.gin.ntt.net  [Akamai Edge]
...
22   242ms  scanme.nmap.org (45.33.32.156)          [Destination]
```

---

### 14. Timing Template

**Purpose**: Faster scan execution

```bash
nmap -T4 scanme.nmap.org
```

**Key Results**: Same ports detected, reduced scan time (8.91s vs 5.81s baseline)

---

## 📊 Summary of Findings

### 🔍 **Discovered Services**
| Port | Service | Version | Notes |
|------|---------|---------|-------|
| 21 | FTP | tcpwrapped | Service detection blocked |
| 22 | SSH | OpenSSH 6.6.1p1 | Ubuntu, multiple key types |
| 80 | HTTP | Apache 2.4.7 | Ubuntu, "ScanMe" title |
| 554 | RTSP | Unknown | Likely streaming service |
| 1723 | PPTP | tcpwrapped | VPN service |
| 9929 | Nping-echo | Nping echo | Nmap testing service |
| 31337 | Elite | tcpwrapped | Legacy hacker port |

### 🛡️ **Filtered Services**
- **SMTP** (25): Mail service blocked
- **NetBIOS/SMB** (135,139,445): Windows services filtered
- **BGP** (179): Routing protocol blocked
- **LDP** (646): Label distribution protocol filtered

### 💻 **Operating System Analysis**
- **Primary Guess**: Linux 4.19-5.15 (93% confidence from -A scan)
- **Alternative**: FreeBSD 6.2-RELEASE (90% from -O scan)
- **Challenge**: Long network distance (23-25 hops) reduces fingerprint accuracy

### 🌐 **Network Characteristics**
- **Latency**: 250-280ms average
- **Path**: India → NTT Backbone → Akamai → Linode datacenter

---

## 🔄 Reproduction Guide

### Prerequisites
```bash
# Install Nmap (if not already installed)
# Windows: Download from https://nmap.org/download.html
# Linux: sudo apt install nmap
# macOS: brew install nmap
```

### Quick Start
```bash
# Clone repository
git clone <your-repo-url>
cd reconnaissance-scanning

# Basic scan
nmap scanme.nmap.org

# Service detection
nmap -sV scanme.nmap.org

# Aggressive scan (comprehensive)
nmap -A scanme.nmap.org
```

### Advanced Techniques
```bash
# Custom port range
nmap -p 1-100 scanme.nmap.org

# UDP scan (selective)
sudo nmap -sU --top-ports 50 scanme.nmap.org

# Script categories
nmap --script=vuln scanme.nmap.org
nmap --script=discovery scanme.nmap.org
```

---

## 🎓 Key Learning Outcomes

### 🔧 **Technical Skills Developed**
- **Nmap Proficiency**: Mastered various scan types and timing options
- **Service Enumeration**: Identified and analyzed network services
- **OS Fingerprinting**: Understanding limitations of remote OS detection
- **Network Analysis**: Interpreted traceroute and latency patterns

### 📈 **Methodology Insights**
- **Scan Progression**: Start with host discovery, then port scanning, finally service detection
- **Timing Considerations**: Balance speed (-T4) vs stealth for different scenarios  
- **Output Management**: Use -oN/-oG/-oX for different analysis needs
- **Script Selection**: Choose NSE scripts based on discovered services

### 🛡️ **Security Implications**
- **Information Disclosure**: Even "tcpwrapped" services reveal port states
- **Fingerprinting Challenges**: Network infrastructure affects OS detection accuracy
- **Service Versions**: Older versions (OpenSSH 6.6.1, Apache 2.4.7) may have vulnerabilities

---

## ⚖️ Ethics & Legal Compliance

### ✅ **Authorized Targets Only**
- `scanme.nmap.org` - Explicitly provided by Nmap project for testing
- `example.com` - Public domain used for documentation/testing
- **Never scan** unauthorized systems or networks

### 📋 **Best Practices**
- Always verify scanning permission before conducting reconnaissance
- Respect rate limits and avoid overwhelming target systems
- Follow responsible disclosure for any vulnerabilities discovered
- Document all activities for audit and learning purposes

### 🏢 **Professional Context**
- Use findings to improve security posture of authorized systems
- Share knowledge responsibly within security community
- Apply learned techniques only in authorized penetration testing scenarios

---

## 📚 References & Further Reading

- [Nmap Official Documentation](https://nmap.org/docs.html)
- [Nmap Scripting Engine (NSE)](https://nmap.org/book/nse.html)
- [Network Scanning Techniques](https://nmap.org/book/scan-methods.html)
- [OS Detection Methods](https://nmap.org/book/osdetect.html)

---
> **Note**: This documentation represents educational reconnaissance activities performed on authorized public targets. All scanning was conducted ethically and within legal boundaries.
