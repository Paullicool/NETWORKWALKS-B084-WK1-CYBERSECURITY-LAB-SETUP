# Cybersecurity Virtual Lab: VirtualBox, Kali Linux, Windows 10 & Android OS

## 1. Project Overview

This project involves building a virtualised cybersecurity laboratory using Oracle VirtualBox to create an isolated environment for cybersecurity learning, network configuration, system administration and authorised security testing.

The lab consists of Kali Linux, Windows 10 and Android OS running as virtual machines. The machines are configured to communicate over a shared VirtualBox NAT Network, allowing internal connectivity while supporting controlled access to external networks where required.

Additional configurations include enabling shared clipboard and drag-and-drop functionality where supported, configuring network adapters and testing connectivity between the virtual machines.

The primary objective is to develop a functional, controlled and repeatable environment for cybersecurity experiments without exposing the host system or external networks to unnecessary risk.

## 2. Project Objectives

The objectives of this project are to:

- Install and configure Oracle VirtualBox as the virtualisation platform.
- Deploy Kali Linux as the primary cybersecurity testing environment.
- Install Windows 10 as a target operating system for authorised security testing.
- Deploy an Android OS environment for mobile security research.
- Configure a shared NAT Network for communication between virtual machines.
- Enable shared clipboard and drag-and-drop features where supported.
- Verify IP addressing, network connectivity and communication between virtual machines.
- Establish a controlled environment for vulnerability assessment, network analysis and security experimentation.
- Document the configuration and validation process for reproducibility.

## 3. Technologies and Tools

|          Technology           |                         Purpose                                                 |

| Oracle VirtualBox             | Virtualisation platform for managing virtual machines |
| Kali Linux                    | Security testing, reconnaissance, network analysis, and vulnerability assessment |
| Windows 10                    | Target operating system for security testing and system administration |
| Android OS                    | Mobile operating system environment for mobile security research |
| VirtualBox NAT Network        | Internal virtual network that supports communication between connected VMs and optional external                                      access |
| VirtualBox Guest Additions    | Additional integration features for supported guest operating systems |
| ADB (Android Debug Bridge)    | Optional tool for authorised Android debugging and testing |
| Command-line networking tools | IP configuration, connectivity testing, and network troubleshooting |

**Note:** Android deployment depends on the selected image. Android-x86, a compatible Android virtual machine image, or an Android emulator can be used. VirtualBox compatibility and Guest Additions support vary by image.

## 4. Lab Architecture

The lab uses a shared NAT Network to connect the virtual machines.

                   Host Computer
                         |
                   Oracle VirtualBox
                         |
                  Virtual NAT Network
                    (CyberLab-NAT)
                         |
             +-----------+-----------+
             |           |           |
          Kali Linux  Windows 10  Android OS
             VM          VM          VM
             |           |           |
             +-----------+-----------+
                Internal Communication
```

### Network design

- **Kali Linux:** Primary security testing and analysis machine.
- **Windows 10:** Target system for authorised security assessment.
- **Android OS:** Mobile security testing environment.
- **CyberLab-NAT:** Shared virtual network connecting all participating machines.
- **Host operating system:** Runs VirtualBox and manages the virtual machines.

A VirtualBox NAT Network differs from the default per-VM NAT mode. A shared NAT Network allows connected virtual machines to communicate with one another while providing outbound network access when configured and available.

The exact IP addresses depend on the NAT Network subnet and DHCP configuration selected during setup.

## 5. Prerequisites

Before beginning the installation, ensure that the host computer meets the following requirements.

### Hardware requirements

- A 64-bit processor supporting hardware virtualisation.
- Sufficient RAM for simultaneous virtual machine operation; 16 GB or more is recommended for a multi-VM lab.
- Adequate SSD storage for operating system images, virtual disks, and snapshots.
- A stable internet connection for downloading operating systems, updates, and tools.

### Software requirements

- Oracle VirtualBox.
- A compatible Kali Linux image.
- A legitimate Windows 10 ISO and valid licensing where applicable.
- A compatible Android OS image or virtual machine.
- Guest Additions or guest integration tools where supported.

Enable Intel VT-x or AMD-V in the computer's firmware settings if hardware virtualisation is disabled.

**Windows 10 lifecycle note:** Standard Windows 10 support ended on 14 October 2025. Use a properly licensed installation, keep the VM isolated, and consider a supported Windows version for ongoing testing where compatibility permits.

## 6. Installation and Configuration

### Step 1: Install Oracle VirtualBox

1. Download VirtualBox from the official website: https://www.virtualbox.org/
2. Select the installer compatible with the host operating system.
3. Run the installer and follow the setup wizard.
4. Install the matching VirtualBox Extension Pack only if required, ensuring that its version matches the installed VirtualBox version.
5. Launch VirtualBox and verify that the application opens successfully.

Official documentation: https://www.virtualbox.org/manual/

### Step 2: Create the NAT Network

Create a shared NAT Network before connecting the virtual machines.

1. Open VirtualBox.
2. Navigate to **Tools → Network** or the NAT Network management section available in your VirtualBox version.
3. Open the NAT Networks tab.
4. Create a new NAT Network.
5. Assign the network a name, such as `CyberLab-NAT`.
6. Configure a private IPv4 subnet, for example `10.10.10.0/24`, after checking that it does not conflict with existing networks.
7. Enable DHCP if automatic IP addressing is required.
8. Enable or activate the NAT Network.
9. Save the configuration.

Configuration:

| Setting               | Example |

| Network name          | CyberLab-NAT |
| IPv4 network          | 10.10.10.0/24 |
| DHCP                  | Enabled |
| Network mode          | NAT Network |
| Intended use          | Communication between lab VMs |



### Step 3: Install Kali Linux

1. Download the appropriate Kali Linux installer from https://www.kali.org/get-kali/.
2. In VirtualBox, select **New** to create a virtual machine.
3. Name the machine `Kali-Linux`.
4. Assign an appropriate amount of RAM and CPU resources based on the host computer's capacity. 
5. Create or attach a virtual disk.
6. Attach the installation ISO.
7. Complete the Kali Linux installation.
8. Install system updates using the package manager.
9. Shut down the VM before changing its network configuration.

Configure the network adapter:

1. Select the Kali VM.
2. Open **Settings → Network**.
3. Enable Adapter 1.
4. Set **Attached to** to `NAT Network`.
5. Select `CyberLab-NAT`.
6. Save the configuration and start the VM.

Verify the network configuration inside Kali:

```bash
ip addr
ip route
ifconfig
```

If DHCP is enabled, confirm that the VM receives an address within the configured subnet.

### Step 4: Install Windows 10

1. Obtain a legitimate Windows 10 ISO from an authorised source.
2. Create a new VM named `Windows-10`.
3. Select an appropriate Windows version and architecture.
4. Allocate suitable RAM, CPU resources, and virtual disk space.
5. Attach the Windows ISO to the virtual optical drive.
6. Start the VM and follow the Windows installation process.
7. Complete the initial setup.
8. Install available updates and required guest integration components.
9. Shut down the VM before configuring the network adapter.

Configure its network:

1. Open **Settings → Network** for the Windows VM.
2. Enable Adapter 1.
3. Select `NAT Network` as the attachment type.
4. Choose `CyberLab-NAT`.
5. Start the VM.

Open Command Prompt and verify the network configuration:

```cmd
ipconfig /all
```

Check that Windows has an IPv4 address in the expected subnet and a valid default gateway.

### Step 5: Install Android OS

Android deployment requires an image compatible with the chosen virtualisation method.

1. Obtain a compatible Android-x86 or other supported Android VM image from its official project source.
2. Create a new VM named `Android-OS`.
3. Select an appropriate operating system type and architecture.
4. Allocate RAM, CPU resources, and a virtual disk according to the image requirements.
5. Attach the Android installation ISO or import the supported image.
6. Follow the image's installation instructions.
7. Boot Android and complete its initial configuration.
8. Shut down the VM before changing network settings.

Configure the network adapter:

1. Open the Android VM's **Settings → Network**.
2. Enable Adapter 1.
3. Select `NAT Network`.
4. Choose `CyberLab-NAT`.
5. Start Android and verify that its network interface is active.

Android images differ in how they expose network settings, obtain IP addresses, and support debugging. If the selected image does not support the expected VirtualBox network configuration, consult its documentation before changing adapter types.

### Step 6: Enable Shared Clipboard and Drag-and-Drop

VirtualBox provides clipboard and drag-and-drop integration for supported guest operating systems.

**For Kali Linux and Windows 10:**

1. Shut down the relevant VM.
2. Select the VM and open **Settings → General → Advanced**.
3. Set **Shared Clipboard** to `Bidirectional`.
4. Set **Drag'n'Drop** to `Bidirectional`.
5. Save the settings.
6. Start the VM and test the features.

For supported Linux guests, install a compatible version of VirtualBox Guest Additions. For Windows guests, install Guest Additions using the VirtualBox Devices menu and the provided installation media.

Guest Additions may be installed from the running VM through:

**Devices → Insert Guest Additions CD Image**

Follow the guest-specific installation instructions and restart the VM if necessary.

**Android limitation:** Shared clipboard and drag-and-drop are not guaranteed to work in Android-x86 or other Android guest images. Their availability depends on guest integration support. If the features are unsupported, use a documented alternative such as ADB for authorised file transfers and debugging.

Only transfer files that are trusted and necessary for the lab.

## 7. Network Connectivity Testing

Once all virtual machines are configured, verify that they can communicate over the shared NAT Network.

### Test 1: Check IP addresses

On Kali Linux:

```bash
ip addr
ip route
ifconfig
```

On Windows 10:

```cmd
ipconfig
```

On Android, inspect the network settings or use supported diagnostic tools.

Record the actual IP address assigned to each VM.

Example addressing table:

| Virtual machine | Example IP address |

| Kali Linux | 10.10.10.10 |
| Windows 10 | 10.10.10.11 |
| Android OS | 10.10.10.12 |

These addresses are examples only. DHCP may assign different addresses, so use the values observed in the lab.

### Test 2: Test connectivity from Kali Linux

Use the actual Windows VM address:

```bash
ping -c 4 <WINDOWS_VM_IP>
```

Test Android connectivity if its networking tools permit:

```bash
ping -c 4 <ANDROID_VM_IP>
```

A successful ping indicates that ICMP traffic is reaching the destination and returning a response. A failed ping does not necessarily mean the network is broken because host firewalls may block ICMP.

### Test 3: Test connectivity from Windows 10

Open Command Prompt:

```cmd
ping <KALI_VM_IP>
ping <ANDROID_VM_IP>
```

Replace the placeholders with the actual IP addresses of the other virtual machines.

If ping fails, check:

- Whether all VMs use the same NAT Network.
- Whether the network is enabled.
- Whether the machines have valid IP addresses.
- Whether their subnet masks and routing configuration are correct.
- Whether the destination firewall blocks ICMP.
- Whether the guest operating system has disabled its network interface.

### Test 4: Test external connectivity

If outbound internet access is required, test DNS resolution and access to an authorised external endpoint.

From Kali Linux:

```bash
getent hosts example.com
```

Then test HTTPS connectivity:

```bash
curl -I https://example.com
```

From Windows:

```cmd
nslookup example.com
```

These tests help distinguish DNS resolution problems from general network connectivity problems.

External access is provided by the NAT Network configuration when enabled and supported by the host network. It should not be assumed to work until tested.

## 8. Validation Checklist

Use this checklist to document the final state of the lab.

- [ ] VirtualBox installed successfully.
- [ ] Kali Linux installed and booting correctly.
- [ ] Windows 10 installed and booting correctly.
- [ ] Android OS installed and booting correctly.
- [ ] `CyberLab-NAT` created and enabled.
- [ ] All VMs attached to the same NAT Network.
- [ ] IP addresses assigned and recorded.
- [ ] Internal connectivity tested between the VMs.
- [ ] DNS resolution tested where required.
- [ ] Outbound connectivity tested where required.
- [ ] Shared clipboard tested on supported guests.
- [ ] Drag-and-drop tested on supported guests.
- [ ] Guest integration tools installed where supported.
- [ ] Firewall behaviour reviewed.
- [ ] Baseline snapshots created.
- [ ] Configuration and test results documented.

Only mark an item as complete after verifying it in the running environment.

## 9. Security Considerations

This lab is designed for controlled, authorised cybersecurity experimentation.

Recommended safeguards include:

- Use isolated lab networks for vulnerability assessment and exploitation exercises.
- Do not use bridged networking unless the experiment specifically requires it and the risks have been assessed.
- Avoid exposing vulnerable guest services to public networks.
- Use snapshots before major configuration changes or potentially destructive experiments.
- Keep host operating system security controls enabled.
- Use trusted operating system images and verify checksums where published.
- Avoid storing personal, confidential, or production data inside the lab.
- Disable shared clipboard and drag-and-drop when testing untrusted samples or when isolation is more important than convenience.
- Do not run malware or exploit code outside an appropriately isolated and authorised environment.
- Obtain permission before testing systems that you do not own or administer.

A NAT Network provides network address translation, but it is not a complete security boundary. Host compromise, guest integration features, exposed services, and misconfiguration can create additional risks.

## 10. Troubleshooting

| Problem | Possible cause | Recommended action |
|---|---|---|
| VM cannot obtain an IP address | DHCP disabled or adapter misconfigured | Check NAT Network settings and DHCP status |
| VMs cannot communicate | Different virtual networks or firewall restrictions | Confirm all VMs use `CyberLab-NAT`; review firewall rules |
| Internet access fails | Host connection, DNS, or NAT configuration issue | Test host connectivity, DNS resolution, and outbound access |
| Clipboard does not work | Guest Additions missing or unsupported | Install compatible Guest Additions and restart |
| Drag-and-drop fails | Guest integration unsupported or misconfigured | Check Guest Additions and guest compatibility |
| Android has no network | Image or virtual NIC compatibility issue | Review the Android image documentation and network configuration |
| Ping fails but internet works | ICMP blocked by firewall | Review firewall rules and use an authorised TCP/HTTPS connectivity test |
| VM runs slowly | Insufficient host resources | Adjust RAM, CPU allocation, and graphics settings |

## 11. Suggested Cybersecurity Experiments

After validating the environment, the lab can support several controlled projects:

1. **Network reconnaissance:** Discover lab hosts and inspect exposed services using Nmap.
2. **Network traffic analysis:** Capture and analyse authorised lab traffic with Wireshark.
3. **Windows security assessment:** Review system configuration, firewall settings, authentication controls, and exposed services.
4. **Linux security assessment:** Examine services, permissions, patch levels, and system logs in Kali Linux.
5. **Android security research:** Explore application permissions, debugging interfaces, and application security using a compatible Android environment.
6. **Network segmentation experiments:** Compare NAT Network behaviour with other VirtualBox networking modes.
7. **Incident response exercises:** Generate benign test events and practise log collection, investigation, and documentation.

All experiments should be conducted only on systems and applications that you own or are explicitly authorised to test.

## 12. Project Outcomes

The completed lab should provide a repeatable virtual environment for practising cybersecurity skills across Linux, Windows, and Android.

Expected outcomes include:

- Practical experience deploying and managing virtual machines.
- Understanding of virtual networking and NAT Network configuration.
- Experience validating connectivity and diagnosing network failures.
- Familiarity with guest integration features and their limitations.
- A foundation for network reconnaissance, vulnerability assessment, traffic analysis, and operating system security testing.
- A documented lab environment suitable for a cybersecurity portfolio.


## 13. Future Improvements

Potential improvements include:

- Adding a dedicated vulnerable Linux target such as Metasploitable in an isolated network.
- Introducing a virtual firewall to practise segmentation and access control.
- Deploying a SIEM platform such as Splunk to collect and analyse lab logs.
- Integrating Wireshark for packet-level traffic analysis.
- Building repeatable test cases for vulnerability assessment and incident response.
- Documenting findings with screenshots, test evidence, and remediation recommendations.
- Maintaining a lab inventory, version history, and network diagram.


## 14. References

- Oracle VirtualBox: https://www.virtualbox.org/
- VirtualBox User Manual: https://www.virtualbox.org/manual/
- Kali Linux Downloads: https://www.kali.org/get-kali/
- Kali Linux Documentation: https://www.kali.org/docs/
- Android-x86 Project: https://www.android-x86.org/
- Android Debug Bridge (ADB): https://developer.android.com/tools/adb
- Microsoft Windows lifecycle information: https://learn.microsoft.com/en-us/lifecycle/products/windows-10-home-and-pro

## 16. Author and Project Information

**Project:** Cybersecurity Virtual Lab Setup and Network Validation

**Focus Areas:** Virtualisation, Network Configuration, Cybersecurity Testing, Linux Administration, Windows Security and Android Security Research.



---

*Disclaimer: This repository documents an educational cybersecurity lab. All security testing must be authorised and restricted to the lab or other systems for which permission has been granted.*
