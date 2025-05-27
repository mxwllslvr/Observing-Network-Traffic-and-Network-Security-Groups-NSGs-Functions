<img src="https://github.com/AnderMendoza/AnderMendoza/raw/main/assets/line-neon.gif" width="100%">

![Github Banner 1 - Creating VMs](https://github.com/user-attachments/assets/41c27c3d-00da-447c-a4c8-79815392d204)

<img src="https://github.com/AnderMendoza/AnderMendoza/raw/main/assets/line-neon.gif" width="100%">

<h1>Observing Network Traffic and Network Security Group (NSG) Functions between Azure Virtual Machines</h1>

In this project, we will observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups.<br/><br/>

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

- Windows 11 Home 24H2
- Windows 10 (22H2)
- Ubuntu Server 20.04 LTS

<h2>Procedure Overview</h2>

- Step 1: Establish RDP Connection to VM and Install Wireshark
- Step 2: Analyze ICMP Traffic
- Step 3: Configure NSG to Block Ping Requests
- Step 4: Examine SSH and DHCP Traffic
- Step 5: Investigate DNS and RDP Traffic

<br/><br/>

<h3>Step 1: Establishing RDP Connection to VM and Installing Wireshark</h3>

<p><img width="850" alt="NSG2" src="https://github.com/user-attachments/assets/93594b30-b3d3-484d-babf-a49c7efce3a9"/>
<br/><br/>
<img width="850" alt="NSG3" src="https://github.com/user-attachments/assets/e20db9e4-591a-45b7-b0f9-db4361c83fbb"/></p>

<p>Access Azure Portal: Log into your Azure account and navigate to the Virtual Machines section. 
<br/><br/>
Start VMs: Select both Windows-VM and Linux-VM by checking their boxes, then click Start. Confirm the action when prompted. Note the Public IP address of Windows-VM for later use.
<br/><br/>
Verify VM Status: After a few minutes, confirm both VMs are running by checking their status in the portal. Minimize the Azure window for now.</p>
<br/><br/>

<p><img width="850" alt="NSG3" src="https://github.com/user-attachments/assets/c8acef54-83c0-4944-93cf-67d34692bdc0"/></p>
<p>Initiate RDP Connection: Open the Run dialog (Windows + R) on your local Windows device and type mstsc to launch the Remote Desktop Connection client. Alternatively, you can search for Remote Desktop Connection in the Start menu.
<br/><br/>
Configure RDP Settings: In the Remote Desktop Connection window, enter the Windows-VM Public IP address in the Computer field. Click show options and enter the username established when provisioning the VM. Click connect, then enter the password we used with that username when the window pops up. Press Ok and establish the connection.</p>
<br/><br/>

<p><img width="850" alt="NSG3" src="https://github.com/user-attachments/assets/bfa3401b-48d7-493c-af56-fe6fc63941d0"/></p>
<p>Install Wireshark: On Windows-VM, launch the Microsoft Edge browser and visit wireshark.org. Download the Windows x64 Installer. Locate the installer in the Downloads folder, run it, and follow the prompts, clicking Next and Finish to complete the installation.
<br/><br/>
Observation: You are now connected to Windows-VM via RDP using mstsc, with Wireshark installed, enabling detailed network traffic analysis.</p>
<br/><br/><br/>

<h3>Step 2: Examining ICMP Traffic</h3>
<p><img width="850" alt="NSG2" src="https://github.com/user-attachments/assets/b26dafaf-ecff-44fd-b32f-009bdf300a07"/></p>
<p>Start Wireshark: Open Wireshark on Windows-VM. Select the Ethernet interface and click the shark fin icon to initiate packet capture.</p>
<br/><br/>

<p><img width="850" alt="NSG2" src="https://github.com/user-attachments/assets/e0508be1-88c5-4256-b156-bb6032bfa200"/></p>
<p>Inspect Initial Traffic: Review the packet list in Wireshark, which displays all active network communications.</p>
<br/><br/>

<p><img width="850" alt="NSG3" src="https://github.com/user-attachments/assets/4afaef41-ae58-4505-93e4-b6f26aad3e0d"/></p>
<p>Filter for ICMP: Enter icmp in Wireshark’s filter bar and press Enter to focus on ICMP (Internet Control Message Protocol) traffic.</p>
<br/><br/>

<p><img width="850" alt="NSG3" src="https://github.com/user-attachments/assets/7d776972-3021-41cf-9a66-66797134c7e0"/></p>
<p>Retrieve Linux VM Private IP: In the Azure Portal, select Linux VM, go to Networking, and note the Private IP address (e.g., 10.0.0.5). Minimize Azure.</p>
<br/><br/>

<p><img width="850" alt="NSG3" src="https://github.com/user-attachments/assets/99b934b4-0d80-4de2-a8ac-1da1838b0738"/></p>
<p>Execute Ping: On Windows-VM, open PowerShell via the Start menu search. Run ping 10.0.0.5 to send ICMP Echo Requests to the Linux VM.
<br/><br/>
Review Results: With the ICMP filter active, Wireshark shows packets from Windows-VM (e.g., 10.0.0.4) to Linux-VM (10.0.0.5) with corresponding replies. PowerShell displays successful ping responses.</p>
<br/><br/>

<p><img width="850" alt="NSG2" src="https://github.com/user-attachments/assets/22920261-8304-4b8e-b04c-f67b4679266f"/></p>
<p>Continuous Ping: In PowerShell, execute ping 10.0.0.5 -t to maintain ongoing ICMP traffic, visible as a steady stream in Wireshark.
<br/><br/>
Observation: Wireshark and PowerShell confirm robust ICMP connectivity between VMs, with continuous request-reply packets during the perpetual ping.</p>
<br/><br/><br/>

<h3>Step 3: Applying NSG Rule to Block Ping Requests</h3>
<p><img width="850" alt="NSG2" src="https://github.com/user-attachments/assets/6a17758a-b938-4f90-93ae-2916ef132e2f"/></p>
<p>Navigate to NSG: In the Azure Portal, go to Virtual Machines, select Linux-VM, and access Networking. Click the Linux-VM-NSG link under Network Security Group.</p>
<br/><br/>

<p><img width="850" alt="NSG3" src="https://github.com/user-attachments/assets/9c753d21-25a9-4991-9346-243d6637b81b"/></p>
<p>Create Inbound Rule: In Inbound security rules, click + Add. Configure the rule as follows:
<br/> &nbsp;&nbsp;&nbsp;&nbsp;  -Source: Any
<br/> &nbsp;&nbsp;&nbsp;&nbsp;  -Source port ranges: * (all ports)
<br/> &nbsp;&nbsp;&nbsp;&nbsp;  -Destination port ranges: * (all ports)
<br/> &nbsp;&nbsp;&nbsp;&nbsp;  -Protocol: ICMPv4 (ping protocol)
<br/> &nbsp;&nbsp;&nbsp;&nbsp;  -Action: Deny
<br/> &nbsp;&nbsp;&nbsp;&nbsp;  -Priority: 290 (to prioritize this rule)
<br/> &nbsp;&nbsp;&nbsp;&nbsp;  -Click Add.</p>




