# 🚀 MPTCP_2025–2026

## Multipath TCP (MPTCP) Implementation and Analysis

🇬🇧 English | [🇫🇷 Français](README.md)

---

## 📌 Project Overview

This repository presents an **academic and technical project** focused on the implementation, analysis, and security of the **Multipath TCP (MPTCP)** protocol in a virtualized Linux environment.

The project aims to **deploy a custom Linux kernel integrating MPTCP**, different from the native implementation available in recent Linux distributions and then analyze the behavioral differences between **standard TCP and MPTCP** communications.

Experiments are conducted using a realistic network setup composed of a **client, a router, and a server**.

This work is carried out as part of the **"Projet Métier"** at **ESAIP**.

---

## 🎯 Project Objectives

* Understand the internal mechanisms of **Multipath TCP (MPTCP)**
* Compile and deploy a **custom MPTCP-enabled Linux kernel**
* Compare **TCP vs MPTCP** network flows
* Analyze protocol behavior and performance
* Study **network, cybersecurity, and resilience aspects**
* Observe MPTCP traffic using **eBPF**
* Design and execute **attack scenarios targeting MPTCP**
* Develop **defensive mechanisms** (route shutdown, subflow control, etc.)

---

## 🧱 Project Architecture

The experimental environment is based on **three virtual machines**:

* 🖥️ **Client**: generates TCP and MPTCP traffic
* 🌐 **Router**: central observation and control point
* 🗄️ **Server**: destination of network flows

### Router role

The router plays a critical role in the project:

* Observation of MPTCP subflows using **eBPF**
* Traffic analysis and monitoring
* Detection of abnormal behaviors
* Application of defensive actions (route cuts, traffic filtering, etc.)

---

## ⚙️ Work Performed

### 🔹 System implementation

* Compilation of a **Linux kernel with MPTCP support**
* Deployment on a virtual machine
* Boot configuration using the custom kernel

### 🔹 Network experiments

* Standard TCP communications
* Multipath TCP communications
* Comparative analysis (latency, resilience, path diversity)

### 🔹 Observation & analysis

* Network traffic capture and inspection
* Analysis of MPTCP subflows
* Router instrumentation using **eBPF**

### 🔹 Security aspects

* Development of scripts to **attack MPTCP communications**
* Network degradation scenarios
* Defensive scripts including:

  * Route shutdown
  * Subflow disruption
  * Dynamic network reactions at router level

---

## 🛠️ Technologies & Tools

* **Linux** Ubuntu 22 LTS
* **Multipath TCP (MPTCP)**
* **Virtual Machines**
* **eBPF**
* **TCP / IP networking**
* Bash and network automation scripts
* Network capture and analysis tools (wireshark...)

---

## 📊 Expected Outcomes

* Clear comparison between TCP and MPTCP behaviors
* Analysis of MPTCP resilience to network path failures
* Real-time observation of protocol behavior using eBPF
* Evaluation of attack and defense strategies

---

## 🎓 Academic Context

* **Institution:** ESAIP
* **Project name:** Projet Métier
* **Supervisor:** Mohammed BENCHEIKH (lecturer and researcher in the field)

### 👥 Project Team

* Benjamin EMEREAU
* Corentin VIGAN
* Mattis LELIÈVRE

---

## 📂 Repository Structure (planned)

```text
MPTCP_2025-2026/
├── docs/           # Documentation, analyses, reports
├── kernel/         # MPTCP kernel configuration and build
├── scripts/        # Attack and defense scripts
├── ebpf/           # eBPF programs
├── topology/       # Network topology and configuration files
└── README.md
```

---

## ⚠️ Disclaimer

This project is conducted **for educational and research purposes only**. All attack scripts are executed strictly within a controlled laboratory environment.

---

## ✍️ Author

Academic project carried out at ESAIP – 2025–2026.
