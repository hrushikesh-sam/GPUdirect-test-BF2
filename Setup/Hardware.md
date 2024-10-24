# Hardware Setup for BlueField-2 DPU (SKU ID: MBF2H322A-AEEOT) - HHHL Configuration

This document provides a step-by-step guide to the hardware setup of the **NVIDIA BlueField-2 DPU** (SKU: **MBF2H322A-AEEOT**) in the Half-Height Half-Length (HHHL) configuration.

## Table of Contents

1. [Introduction](#introduction)
2. [Package Contents](#package-contents)
3. [Hardware Requirements](#hardware-requirements)
4. [Installation Overview](#installation-overview)
5. [Detailed Installation Steps](#detailed-installation-steps)
    1. [Step 1: Ensure System Compatibility](#step-1-ensure-system-compatibility)
    2. [Step 2: Unboxing and Inspection](#step-2-unboxing-and-inspection)
    3. [Step 3: Prepare the Host System](#step-3-prepare-the-host-system)
    4. [Step 4: Install the BlueField-2 DPU](#step-4-install-the-bluefield-2-dpu)
    5. [Step 5: Connect the Network Interface](#step-5-connect-the-network-interface)
    6. [Step 6: Boot and Verify](#step-6-boot-and-verify)
6. [Post-Installation Setup](#post-installation-setup)
7. [Troubleshooting](#troubleshooting)
8. [Resources and Documentation](#resources-and-documentation)

---

## Introduction

The **NVIDIA BlueField-2 DPU** is a high-performance Data Processing Unit designed for efficient offloading, acceleration, and security functions in data center environments. The SKU **MBF2H322A-AEEOT** is specifically designed for Half-Height Half-Length (HHHL) server configurations.

## Package Contents

Ensure that you have received the following components:

1. **BlueField-2 DPU Card (MBF2H322A-AEEOT)** in HHHL form factor.
2. **Low-Profile Bracket** (pre-installed).
3. **Full-Height Bracket** (optional, if needed for 2U chassis).
4. **Documentation and Warranty Information**.
5. **Screws and Mounting Accessories** (if applicable).

If any items are missing or damaged, contact NVIDIA support or your reseller.

## Hardware Requirements

- **Host System**: Ensure your server or workstation has an available PCIe slot that supports the BlueField-2 DPU. The SKU MBF2H322A-AEEOT uses a PCIe 4.0 x8 interface.
- **PCIe Slot**: Available x8 or x16 PCIe slot (PCIe Gen3 or Gen4 compatible).
- **Operating System**: Ensure the operating system supports the BlueField-2 DPU. You may need to install Mellanox drivers or the DOCA SDK for full functionality.
- **Network Cables**: The DPU supports dual-port 25GbE/10GbE, so ensure compatible SFP28 or SFP+ transceivers or DACs (Direct Attach Cables).

## Installation Overview

The installation process involves:
1. Ensuring system compatibility.
2. Unboxing and inspecting the DPU.
3. Preparing the host system.
4. Physically installing the DPU in the PCIe slot.
5. Connecting the network interfaces.
6. Booting the system and verifying the DPU’s functionality.

## Detailed Installation Steps

### Step 1: Ensure System Compatibility

- Ensure your server or workstation has an available PCIe slot (x8 or x16) and enough physical space for an HHHL card.
- Verify the server's power supply can support the additional power requirements of the BlueField-2 DPU.
- Check that the host system firmware and BIOS are up to date to avoid any compatibility issues.

### Step 2: Unboxing and Inspection

1. Carefully unbox the BlueField-2 DPU and all its accessories.
2. Inspect the card for any physical damage. Check connectors and components.
3. Make sure the low-profile bracket is installed (if required for your chassis), or replace it with the full-height bracket if needed for a standard server rack.

### Step 3: Prepare the Host System

1. Power off the host system.
2. Unplug all cables (power, network, etc.) from the system.
3. Open the chassis by removing the cover or side panel to expose the PCIe slots.

### Step 4: Install the BlueField-2 DPU

1. **Locate the PCIe Slot**: Find an available x8 or x16 PCIe slot in your server. The DPU requires a PCIe 4.0-compatible slot for optimal performance, but it can also work in PCIe Gen3 slots.
2. **Insert the Card**: Gently align the DPU with the PCIe slot and press it down firmly until it is seated properly in the slot.
3. **Secure the Card**: Use screws or clips to secure the DPU to the chassis, ensuring it doesn't move during operation.

### Step 5: Connect the Network Interface

1. If using fiber optics, insert the SFP28 or SFP+ transceivers into the ports.
2. If using DAC cables, connect the appropriate cables to the DPU’s network ports.
4. Make sure you connect an ethernet the OOB port for software installation.
3. Make sure the cables are routed properly to avoid blocking airflow or interfering with other components.

### Step 6: Boot and Verify

1. Reassemble the chassis by replacing the cover or side panel.
2. Plug in the power and reconnect all necessary cables (keyboard, monitor, network).
3. Power on the system.
4. Access the BIOS/UEFI setup to verify that the DPU is detected in the PCIe slot.
5. Boot into the operating system.
6. After boot, verify that the DPU is detected by the operating system using commands like `lspci` (Linux) or `Device Manager` (Windows).
   ```bash
    lspci | grep -i Mellanox
   ```

## Post-Installation Setup

After the hardware is set up, install the necessary drivers and firmware. For full DPU functionality, you may need to install:

- **DOCA SDK** for DPU-specific applications and management.

Refer to NVIDIA’s official website for software downloads and further setup instructions.

## Troubleshooting

If the system does not detect the DPU or if there are any issues during installation:

1. **Check PCIe Slot**: Ensure that the DPU is seated correctly in the PCIe slot.
2. **Verify Power**: Ensure that the power supply is sufficient and that the card is receiving power.
3. **Firmware Update**: Make sure the system’s BIOS and DPU firmware are up to date.
4. **Driver Installation**: Verify that the correct drivers are installed for your operating system.

## Resources and Documentation

- **NVIDIA BlueField-2 Setup Guide**: [Link](https://docs.nvidia.com/nvidia-bluefield-2-ethernet-dpu-user-guide.pdf)
- **DOCA SDK Documentation**: [Link](https://docs.nvidia.com/doca/sdk/nvidia+doca+installation+guide+for+linux/index.html)




