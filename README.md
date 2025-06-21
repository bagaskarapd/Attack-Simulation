# Home Lab Attack Simulation & Telemetry Generation
<p>This project was designed as a simulation of a real-world cyberattack, conducted in a virtual lab environment. Its primary goal is to demonstrate how endpoint telemetry can be generated, collected, and analyzed using widely available tools such as Sysmon and Splunk. This project helped me gain hands-on experience with attacker techniques, logging mechanisms, and security monitoring—all of which are essential for working in a SOC environment.</p>

# Tools Used
<ul>
  <li>Kali Linux – As the attacker machine</li>
  <li>Windows 10 – As the target machine</li>
  <li>Sysmon – To capture detailed telemetry data</li>
  <li>Splunk – As the SIEM for log analysis</li>
  <li>Metasploit & MSFVenom – For payload generation and reverse shell</li>
  <li>Python HTTP Server – For malware delivery</li>
  <li>Nmap – For reconnaissanceNmap – For reconnaissance</li>
</ul>

# Simulation
<p>To set up the lab, I used VirtualBox to run two virtual machines: Kali Linux (as the attacker) and Windows 10 (as the target). Both VMs were configured to use an "Internal Network" mode in VirtualBox to ensure isolated communication within the same virtual network. This configuration prevents interaction with the outside internet while allowing both machines to communicate securely.</p>

<p align="center">
    <img src="https://github.com/bagaskarapd/Attack-Simulation/blob/main/Screenshots/%F0%9F%9B%A1%EF%B8%8F%20Home%20Lab%20Attack%20Simulation%20&%20Telemetry%20Generation%20-%20visual%20selection.png?raw=true">
</p>

<p>To ensure stable communication and easier referencing, each virtual machine was assigned a static IP address. Kali Linux was configured with an IP address such as 192.168.56.100, while Windows 10 was assigned 192.168.56.110. These IPs fall within the same subnet and enable consistent access throughout the simulation. This setup ensures that tools like Nmap and Metasploit can reliably interact with the Windows VM during reconnaissance and exploitation.</p>

<p>The simulation begins with reconnaissance. Using Kali Linux, I performed a network scan with Nmap to discover open ports and services on the Windows target. This scan revealed active services, including the Remote Desktop Protocol (RDP) port 3389. With this information, I moved on to crafting a simple reverse shell payload using msfvenom. The payload was configured to connect back to the Kali machine’s static IP, allowing full control of the victim upon execution.</p>
<p align="center">
    <img src="https://github.com/bagaskarapd/Attack-Simulation/blob/main/Screenshots/%F0%9F%9B%A1%EF%B8%8F%20Home%20Lab%20Attack%20Simulation%20&%20Telemetry%20Generation%20-%20visual%20selection.png?raw=true">
</p>
