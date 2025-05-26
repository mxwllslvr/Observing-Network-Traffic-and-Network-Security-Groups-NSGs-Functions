<img src="https://github.com/AnderMendoza/AnderMendoza/raw/main/assets/line-neon.gif" width="100%">

![Github Banner 1 - Creating VMs](https://github.com/user-attachments/assets/41c27c3d-00da-447c-a4c8-79815392d204)

<img src="https://github.com/AnderMendoza/AnderMendoza/raw/main/assets/line-neon.gif" width="100%">

<h1>Observing Network Traffic and Network Security Group (NSG) Functions between Azure Virtual Machines</h1>

In this project, we will observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />

<h2>🚨Prerequisites🚨</h2>

- [Creating Virtual Machines in the Cloud](https://github.com/mxwllslvr/creating-virtual-machines-in-the-cloud)
  
<h2>Technologies Utilized</h2>

- Microsoft Azure (Compute/Virtual Machines)
- Remote Desktop Protocol (RDP)
- Windows App (macOS)
- PowerShell
- Network Protocols (ICMP, SSH, DHCP, DNS, RDP)
- Wireshark (Network Protocol Analyzer)

<h2>Operating Systems</h2>

- macOS Sequoia
- Windows 10 (22H2)
- Ubuntu Server 20.04 LTS

<h2>Procedure Overview</h2>

- Step 1: Establish RDP Connection to VM and Install Wireshark
- Step 2: Analyze ICMP Traffic
- Step 3: Configure NSG to Block Ping Requests
- Step 4: Examine SSH and DHCP Traffic
- Step 5: Investigate DNS and RDP Traffic

<h3>Step 1: Establishing RDP Connection to VM and Installing Wireshark</h3>
<p>
<img width="850" alt="NSG2" src="https://github.com/user-attachments/assets/93594b30-b3d3-484d-babf-a49c7efce3a9"/>
 
<img width="850" alt="NSG3" src="https://github.com/user-attachments/assets/e20db9e4-591a-45b7-b0f9-db4361c83fbb"/>
 </p>
<p>
  Access Azure Portal: Log into your Azure account and navigate to the Virtual Machines section. 

  Start VMs: Select both windows-vm and linux-vm by checking their boxes, then click Start. Confirm the action when prompted. Note the Public IP address of windows-vm for later use.

  Verify VM Status: After a few minutes, confirm both VMs are running by checking their status in the portal. Minimize the Azure window for now.
</p>
<br />





