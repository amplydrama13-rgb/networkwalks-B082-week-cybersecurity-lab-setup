# Kali Linux VirtualBox Network Setup & Fix Guide

A step-by-step guide to configuring a static **NAT Network** in VirtualBox for Kali Linux and resolving NetworkManager connection delay issues caused by Duplicate Address Detection (DAD) timeouts.

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

---

### 3. Fix Connection Delay / DAD Timeout (CLI Fix)

If your network connection takes a long time to establish or hangs during startup, disable the **IPv4 Duplicate Address Detection (DAD) timeout** using `nmcli`:

```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
