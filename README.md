
# 🔐 Raspberry Pi Pentesting Project

This repository documents penetration testing performed on a Raspberry Pi device.  
All commands and examples below are taken from the project progress reports (Raspberry pi pentesting.pdf).  
> ⚠️ **For educational use in a controlled lab only. Do not run these commands on systems or networks you do not own or have explicit permission to test.**

---

## Table of Contents

- [Setup](#setup)  
- [Brute-force attack (Hydra)](#brute-force-attack-hydra)  
- [Man-in-the-Middle (MITM) attacks](#man-in-the-middle-mitm-attacks)  
  - [Bettercap](#bettercap)  
  - [Ettercap](#ettercap)  
  - [Wireshark](#wireshark)  
- [System exploitation & bootloader manipulation](#system-exploitation--bootloader-manipulation)  
  - [Network discovery](#network-discovery)  
  - [SSH access & destructive commands](#ssh-access--destructive-commands)  
  - [Boot delay injection & file editing](#boot-delay-injection--file-editing)  
- [Overclocking & hardware impact](#overclocking--hardware-impact)  
- [Results & recommendations](#results--recommendations)  
- [Authors](#authors)

---

## Setup

1. Flash and install the 64-bit Raspbian operating system to an SD card using an SD card reader.  
2. Connect the Raspberry Pi (Raspberry Pi 4 Model B) to a monitor, keyboard, and power.  
3. Use a Kali Linux virtual machine (VMware) to perform the penetration tests.

---

## Brute-force attack (Hydra)

Create `username` and `password` lists (two text documents). Example files: `username.txt` and `password.txt` (these files included the correct credentials used in the test: `CSE` / `CSE`).

Run Hydra against SSH:
```bash
hydra -L <path_to_username_list> -P <path_to_password_list> ssh://<target_ip>
