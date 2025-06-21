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
<p>In the simulation, I used Kali Linux as the attacker machine and Windows 10 as the victim, both running inside VirtualBox. The Windows machine was configured with Sysmon to monitor detailed system-level events like process creation, network connections, and command-line executions. These logs were ingested into Splunk, a powerful SIEM tool, to allow real-time searching and investigation of suspicious activities.</p>

<p align="center">
    <img src="https://github.com/bagaskarapd/Attack-Simulation/blob/main/Screenshots/%F0%9F%9B%A1%EF%B8%8F%20Home%20Lab%20Attack%20Simulation%20&%20Telemetry%20Generation%20-%20visual%20selection.png?raw=true">
</p>

<p>The attack process begins with reconnaissance using Nmap to identify open ports and available services on the Windows machine.</p>
<p align="center">
    <img src="https://github.com/bagaskarapd/Attack-Simulation/blob/main/Screenshots/%F0%9F%9B%A1%EF%B8%8F%20Home%20Lab%20Attack%20Simulation%20&%20Telemetry%20Generation%20-%20visual%20selection.png?raw=true">
</p>
