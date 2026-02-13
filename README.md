# TASD-IDD: Truly Authentic Software Defined Intrusion Detection Dataset

**TASD-IDD** is a realistic Software-Defined Networking (SDN) intrusion detection dataset designed to address the lack of publicly available *raw* datasets in the SDN security landscape. Unlike traditional datasets that rely on PCAP captures at endpoints, TASD-IDD collects flow statistics directly from the **ONOS controller's Northbound Interface (NBI)**, providing a global network view authentic to SDN environments.

## 📋 Dataset Overview

* **Paper Title:** TASD-IDD: Truly Authentic Software Defined Intrusion Detection Dataset
* **Conference:** 2025 IEEE International Conference on Networking and Advanced Systems (ICNAS)
* **Authors:** Ishak Serrat, Tayeb Kenaza, Chaouki Behaz,  Oussama Choudar, Islam Debicha
* **Controller:** ONOS v2.6.0
* **Format:**
    * **Raw Data:** CSV files containing direct flow rule statistics from the controller.
    * **Processed Data:** CSV files with **64 attributes** derived using a bidirectional flow extraction method.

## 🎯 Motivation

Existing SDN datasets often rely on PCAP files captured at host machines, which does not fully align with the SDN paradigm where security applications (App Plane) monitor the network via the Controller. TASD-IDD leverages the **ONOS REST API** to collect statistics, offering:
1.  **True SDN Abstraction:** Data reflects what an SDN Application actually "sees" (Flow Rules, not Packets).
2.  **Raw Data Availability:** Enables researchers to perform custom feature engineering.
3.  **Bidirectional Flow Analysis:** Features are calculated by correlating forward and backward flows.

## 🏗️ Testbed Topology

The dataset was generated in a hybrid environment combining emulated and physical components:
* **Controller:** ONOS v2.6.0
* **Switches:** 6 Open vSwitch (OVS) instances running OpenFlow 1.4 (4 inside Mininet, 2 physical).
* **Hosts:** 10 total nodes.
    * **Attackers (3):** Kali Linux VM, 2 Ubuntu Mininet hosts.
    * **Victims (7):** Ubuntu instances, Metasploitable2 VM, Web Server (DVWA/SimpleRisk), FTP Server.

## ⚔️ Attack Scenarios & Traffic Types

The dataset contains a balanced mix of legitimate and malicious traffic generated within the testbed.

### ✅ Benign Traffic
1.  **ICMP:** Ping commands with varying rates and packet lengths.
2.  **Web Browsing:** Manual browsing sessions on a SimpleRisk web application (30-minute sessions).
3.  **File Transfer:** FTP uploads/downloads ranging from 100MB to 10GB using Python `ftplib`.
4.  **Video Streaming:** HD video streaming using FFMPEG (unicast).

### ❌ Malicious Traffic
1.  **Network Scanning:**
    * Port Scanning (`nmap -p`)
    * OS Detection (`nmap -O`)
2.  **DoS (Denial of Service):**
    * SYN Flood (using Scapy)
    * ICMP Flood (using Hping3)
    * UDP Flood (using Hping3)
3.  **DDoS (Distributed DoS):** Randomized source ICMP floods from multiple attackers targeting a single victim.
4.  **Web Attacks:** SQL Injection and XSS Injection targeting DVWA.
5.  **Password Guessing:** SSH Brute-force attacks using Medusa (targeting port 22 with Rockyou dictionary).

## 📂 Data Structure

### 1. Raw Data
Collected via polling the ONOS API every 2 seconds. Contains raw flow rules including:
* **Identifiers:** `flow_id`, `device_id`, `tableId`
* **Match Fields:** `eth_type`, `ip_proto`, `ipv4_src`, `ipv4_dst`, `port_src`, `port_dst`, `mac_src`, `mac_dst`
* **Counters:** `tot_pkts`, `tot_bytes`
* **Flow Properties:** `duration`, `timeout`, `priority`, `timestamp`

### 2. Processed Data (Features)
A derived dataset ready for Machine Learning, containing **64 attributes**. It uses a bidirectional approach where forward and backward flows of a session are merged to calculate complex features suitable for AI model training.

## 📝 Citation

If you use this dataset in your research, please cite the following paper:

```bibtex
@inproceedings{serrat2025tasd,
  title={TASD-IDD: Truly Authentic Software Defined Intrusion Detection Dataset},
  author={Serrat, Ishak and Kenaza, Tayeb and Choudar, Oussama and Behaz, Chaouki and Debicha, Islam},
  booktitle={2025 International Conference on Networking and Advanced Systems (ICNAS)},
  pages={1--8},
  year={2025},
  organization={IEEE}
}
