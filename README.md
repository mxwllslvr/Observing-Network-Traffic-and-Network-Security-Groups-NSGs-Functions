<img src="https://github.com/AnderMendoza/AnderMendoza/raw/main/assets/line-neon.gif" width="100%">

![Github Banner 1 - Creating VMs](https://github.com/user-attachments/assets/41c27c3d-00da-447c-a4c8-79815392d204)

<img src="https://github.com/AnderMendoza/AnderMendoza/raw/main/assets/line-neon.gif" width="100%">

# Observing Network Traffic and Network Security Group (NSG) Functions between Azure Virtual Machines

In this project, we will observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups.

## 🚨 Prerequisites

- [Creating Virtual Machines in the Cloud](https://github.com/mxwllslvr/creating-virtual-machines-in-the-cloud)

## Technologies Utilized

- Microsoft Azure (Compute/Virtual Machines)
- Remote Desktop Protocol (RDP) via mstsc
- PowerShell
- Network Protocols (ICMP, SSH, DHCP, DNS, RDP)
- Wireshark (Network Protocol Analyzer)

## Operating Systems

- Windows 11 Home 24H2
- Windows 10 (22H2)
- Ubuntu Server 20.04 LTS

## Procedure Overview

1. Connect to VM via RDP Using mstsc and Install Wireshark
2. Examine ICMP Traffic
3. Apply NSG Rule to Block Ping Requests
4. Analyze SSH and DHCP Traffic
5. Evaluate DNS and RDP Traffic

---

## Step 1: Connect to VM via RDP Using mstsc and Install Wireshark

### Access Azure Portal and Start VMs

![Azure Portal VM Management](https://github.com/user-attachments/assets/93594b30-b3d3-484d-babf-a49c7efce3a9)

![VM Selection](https://github.com/user-attachments/assets/e20db9e4-591a-45b7-b0f9-db4361c83fbb)

1. **Access Azure Portal**: Log into your Azure account and navigate to the Virtual Machines section
2. **Start VMs**: Select both **Windows-VM** and **Linux-VM** by checking their boxes, then click **Start**. Confirm the action when prompted. Note the Public IP address of **Windows-VM** for later use
3. **Verify VM Status**: After a few minutes, confirm both VMs are running by checking their status in the portal. Minimize the Azure window for now

### Establish RDP Connection

![RDP Connection Setup](https://github.com/user-attachments/assets/c8acef54-83c0-4944-93cf-67d34692bdc0)

1. **Initiate RDP Connection**: Open the Run dialog (**Windows + R**) on your local Windows device and type `mstsc` to launch the Remote Desktop Connection client. Alternatively, you can search for **Remote Desktop Connection** in the Start menu
2. **Configure RDP Settings**: In the Remote Desktop Connection window, enter the **Windows-VM Public IP address** in the Computer field. Click **Show Options** and enter the username established when provisioning the VM. Click **Connect**, then enter the password when the window appears. Press **OK** to establish the connection

### Install Wireshark

![Wireshark Installation](https://github.com/user-attachments/assets/bfa3401b-48d7-493c-af56-fe6fc63941d0)

1. **Install Wireshark**: On **Windows-VM**, launch the Microsoft Edge browser and visit [wireshark.org](https://wireshark.org)
2. Download the **Windows x64 Installer**
3. Locate the installer in the Downloads folder, run it, and follow the prompts, clicking **Next** and **Finish** to complete the installation

**Observation**: You are now connected to **Windows-VM** via RDP using mstsc, with Wireshark installed, enabling detailed network traffic analysis.

---

## Step 2: Examine ICMP Traffic

### Start Wireshark and Begin Packet Capture

![Wireshark Interface](https://github.com/user-attachments/assets/b26dafaf-ecff-44fd-b32f-009bdf300a07)

Open Wireshark on **Windows-VM**. Select the **Ethernet** interface and click the shark fin icon to initiate packet capture.

### Inspect Initial Traffic

![Initial Traffic](https://github.com/user-attachments/assets/e0508be1-88c5-4256-b156-bb6032bfa200)

Review the packet list in Wireshark, which displays all active network communications.

### Filter for ICMP Traffic

![ICMP Filter](https://github.com/user-attachments/assets/4afaef41-ae58-4505-93e4-b6f26aad3e0d)

Enter `icmp` in Wireshark's filter bar and press **Enter** to focus on ICMP (Internet Control Message Protocol) traffic.

### Retrieve Linux VM Private IP

![Linux VM IP](https://github.com/user-attachments/assets/7d776972-3021-41cf-9a66-66797134c7e0)

In the Azure Portal, select **Linux VM**, go to **Networking**, and note the Private IP address (e.g., 10.0.0.5). Minimize Azure.

### Execute Ping Commands

![Ping Execution](https://github.com/user-attachments/assets/99b934b4-0d80-4de2-a8ac-1da1838b0738)

1. On **Windows-VM**, open PowerShell via the Start menu search
2. Run `ping 10.0.0.5` to send ICMP Echo Requests to the Linux VM
3. **Review Results**: With the ICMP filter active, Wireshark shows packets from **Windows-VM** (e.g., 10.0.0.4) to **Linux-VM** (10.0.0.5) with corresponding replies. PowerShell displays successful ping responses

### Continuous Ping Test

![Continuous Ping](https://github.com/user-attachments/assets/22920261-8304-4b8e-b04c-f67b4679266f)

1. In PowerShell, execute `ping 10.0.0.5 -t` to maintain ongoing ICMP traffic
2. This appears as a steady stream in Wireshark

**Observation**: Wireshark and PowerShell confirm robust ICMP connectivity between VMs, with continuous request-reply packets during the perpetual ping.

---

## Step 3: Apply NSG Rule to Block Ping Requests

### Navigate to Network Security Group

![NSG Navigation](https://github.com/user-attachments/assets/6a17758a-b938-4f90-93ae-2916ef132e2f)

In the Azure Portal, go to **Virtual Machines**, select **Linux-VM**, and access **Networking**. Click the **Linux-VM-NSG** link under Network Security Group.

### Create Inbound Security Rule

![Create NSG Rule](https://github.com/user-attachments/assets/9c753d21-25a9-4991-9346-243d6637b81b)

In **Inbound security rules**, click **+ Add**. Configure the rule as follows:

- **Source**: Any
- **Source port ranges**: * (all ports)
- **Destination port ranges**: * (all ports)
- **Protocol**: ICMPv4 (ping protocol)
- **Action**: Deny
- **Priority**: 290 (to prioritize this rule)

Click **Add**.

### Confirm Rule Creation

![Rule Confirmation](https://github.com/user-attachments/assets/0ade2ac7-8802-414c-9c54-7475ca72a4d1)

Verify the new rule appears at the top of the inbound rules list.

### Observe Ping Disruption

![Ping Blocked](https://github.com/user-attachments/assets/8c4f914e-54ce-404e-a8da-dfcf592b674c)

Return to **Windows-VM**. In PowerShell, note the perpetual ping (`ping 10.0.0.5 -t`) now reports "Request timed out". Wireshark marks the timestamp when ICMP replies stop, reflecting the NSG rule's effect.

### Delete the NSG Rule

![Delete Rule](https://github.com/user-attachments/assets/5917fb5a-2b40-4b96-b0e0-8f68237c980b)

In Azure, revisit the NSG's **Inbound security rules**, select the new rule, and click the trash can icon to remove it. Confirm deletion.

### Verify Ping Restoration

![Ping Restored](https://github.com/user-attachments/assets/21574ba9-f455-4c11-8d4f-13d7f2b165c9)

1. On **Windows-VM**, confirm in PowerShell that ping replies resume (e.g., "Reply from 10.0.0.5")
2. Wireshark displays ICMP request-reply packets again
3. Terminate the perpetual ping with **Ctrl + C** in PowerShell

**Observation**: The NSG rule successfully blocked ICMP traffic, interrupting pings, and its removal restored connectivity, illustrating NSG's firewall capabilities.

---

## Step 4: Analyze SSH and DHCP Traffic

### Capture SSH Traffic

![SSH Traffic](https://github.com/user-attachments/assets/99457421-b701-4209-b961-1cb4bec4e8ac)

1. In Wireshark, enter `ssh` in the filter bar and start a new capture
2. In PowerShell, establish an SSH connection to **Linux-VM** with `ssh username@10.0.0.5` (substitute username with **Linux-VM's** username)
3. Type `yes` to accept the connection prompt, then enter the password (the input field remains blank during entry)
4. Wireshark displays encrypted SSH packets, confirming a secure connection between VMs
5. In PowerShell, type `exit` to terminate the session, observing the "Connection to 10.0.0.5 closed" message

### Capture DHCP Traffic

![DHCP Traffic](https://github.com/user-attachments/assets/662fb38f-be35-400a-adb0-c88f25a66a7f)

1. In Wireshark, enter `dhcp` in the filter bar and start a new capture
2. In PowerShell, run `ipconfig /renew` to request an IP lease. Note limited traffic due to the VM's static IP configuration
3. To trigger DHCP activity, create a batch file:
   - Open Notepad on **Windows-VM** and enter:
     ```batch
     ipconfig /release
     ipconfig /renew
     ```
   - Press **Windows + E** to access Explorer, and navigate to **C:\** folder
   - Click **View**, make sure **Hidden Items** is checked. Refresh the folder
   - Save the Notepad file as `dhcp.bat` in the newly accessible **C:\ProgramData**

![DHCP Batch Execution](https://github.com/user-attachments/assets/9b56eafc-7928-4d6c-a81f-2ba439d0a883)

4. In PowerShell, navigate with `cd C:\ProgramData`, list files with `ls`, and execute the batch file with `.\dhcp.bat`
5. Observe a brief disconnection during `ipconfig /release`, followed by reconnection via `ipconfig /renew`
6. Wireshark captures DHCP packets (e.g., Discover, Offer, Request, ACK)

**Observation**: SSH traffic reveals encrypted communication, while the batch file generates observable DHCP lease negotiations, demonstrating IP assignment dynamics.

---

## Step 5: Evaluate DNS and RDP Traffic

### Capture DNS Traffic

![DNS Traffic](https://github.com/user-attachments/assets/3cc852e6-364b-45da-8fce-a14d27ab5f12)

1. In Wireshark, enter `dns` in the filter bar and start a new capture
2. In PowerShell, execute `nslookup disney.com` to resolve Disney's IP address (e.g., 130.211.198.204)
3. In Edge, navigate to the IP address. Note a Disney-related page (e.g., with a logo or error), indicating restricted access but confirming DNS's role in mapping domain names to IPs

### Capture RDP Traffic

![RDP Traffic](https://github.com/user-attachments/assets/c7178afb-b78e-4513-9daf-135555d42705)

1. In Wireshark, enter `tcp.port == 3389` in the filter bar to isolate RDP traffic, then start a new capture
2. Observe ongoing packet flow, reflecting the active RDP session to **Windows-VM** maintained throughout the exercise
3. Alternatively, filter with `rdp` to view similar traffic, verifying the protocol's continuous network activity during remote access

**Observation**: DNS traffic illustrates domain-to-IP resolution, while RDP traffic highlights the persistent network activity of remote connections, evident since the session's start.

---

<br/><br/>

<div align="center"> <img src="https://media0.giphy.com/media/v1.Y2lkPTc5MGI3NjExYTVlM2J0cDBrcThwOTR3cm45bndrYWNycDNneWVuNDIzeGM0ZDB4ZiZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/jir4LEGA68A9y/giphy.gif" width="75%"> </div>

<br/><br/>

<img src="https://github.com/AnderMendoza/AnderMendoza/raw/main/assets/line-neon.gif" width="100%">

## Conclusion

This procedure successfully connected to **Windows-VM** via RDP using mstsc, installed Wireshark, and analyzed ICMP, SSH, DHCP, DNS, and RDP traffic using Wireshark and PowerShell. By implementing an NSG rule to block ICMP traffic, we demonstrated fundamental cybersecurity principles, observing the immediate effects of firewall policies.

**Key Learning Outcomes:**
- Network protocol analysis using Wireshark
- Azure Network Security Group configuration and testing
- Understanding of various network protocols (ICMP, SSH, DHCP, DNS, RDP)
- Practical experience with network troubleshooting tools

This exercise underscores the capabilities of network analysis tools and Azure's security infrastructure. To manage costs, stop both VMs in Azure when idle. This concludes the procedure, equipping you with essential skills for network monitoring and security configuration.

<img src="https://github.com/AnderMendoza/AnderMendoza/raw/main/assets/line-neon.gif" width="100%">

<br/><br/>

<div align="center">

<a href="HTTP://www.github.com/mxwllslvr">- BACK TO MAIN PAGE -</a>

</div>

