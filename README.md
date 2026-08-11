# Kali Linux VirtualBox Network Setup & Fix Guide

A step-by-step guide to configuring a static **NAT Network** in VirtualBox for Kali Linux and resolving NetworkManager connection delay issues caused by Duplicate Address Detection (DAD) timeouts.
<img width="1364" height="645" alt="kali" src="https://github.com/user-attachments/assets/c88a04f6-2650-4fb9-b78b-5eb9a925a3aa" />

---

## 🛠️ Configuration Overview

| Parameter | Recommended Value |
| :--- | :--- |
| **Network Type** | VirtualBox NAT Network |
| **IPv4 Subnet** | `10.0.0.0/24` |
| **Static IP Address** | `10.0.0.2` |
| **Netmask** | `24` (`255.255.255.0`) |
| **Gateway** | `10.0.0.1` |
| **DNS Server** | `8.8.8.8` |
<img width="1365" height="666" alt="screenshot network settings" src="https://github.com/user-attachments/assets/11c6bdc3-1bac-4137-b3c4-8c4b917583f9" />

---

## 🚀 Setup Steps

### 1. VirtualBox NAT Network Configuration

1. Open VirtualBox and navigate to **Tools** > **Network Manager** > **NAT Networks**.
2. Click **Create** to add a new NAT Network (e.g., `NatNetwork`).
3. Set the following parameters:
   * **IPv4 Prefix:** `10.0.0.0/24`
   * **Enable DHCP:** `Checked`
4. Apply the settings and attach this network interface to your Kali Linux virtual machine under **VM Settings** > **Network** > **Attached to: NAT Network**.

---

### 2. Manual Network Configuration in Kali Linux GUI

1. Open **NetworkManager** settings in Kali Linux (click the network icon in the top panel).
2. Edit your wired connection (e.g., `Wired connection 1` or `Wired connection 2`).
3. Select the **IPv4 Settings** tab:
   * **Method:** `Manual`
   * **Address:** `10.0.0.2`
   * **Netmask:** `24`
   * **Gateway:** `10.0.0.1`
   * **DNS servers:** `8.8.8.8`
4. Click **Save** and apply the changes.
<img width="1363" height="644" alt="Kali manual network configuration" src="https://github.com/user-attachments/assets/0d15a90b-6dac-48f7-96c0-d652b9021b6d" />

---

### 3. Fix Connection Delay / DAD Timeout (CLI Fix)

If your network connection takes a long time to establish or hangs during startup, disable the **IPv4 Duplicate Address Detection (DAD) timeout** using `nmcli`:
<img width="1365" height="643" alt="command to fix If you face " src="https://github.com/user-attachments/assets/318d91ea-911c-415a-ba88-189698141a30" />

```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0

