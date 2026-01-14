# Snort
This snort project demonstrates Active Response and IPS (Intrusion Prevention System) configuration.

<h2>Project Overview</h2>
<p>In this incident response scenario, I acted as a Security Engineer for "J&Y Enterprise" to defend a high-value asset against an active Brute-Force attack. Using Snort, I transitioned from passive network sniffing to active intrusion prevention. I identified the attack vector, analyzed the malicious traffic patterns, and authored a custom IPS rule to drop the unauthorized traffic in real-time.</p> </br>

<h2>Technical Skills Demonstrated</h2>
<p>
<b>IPS Rule Authoring:</b> Writing custom Snort signatures using correct syntax (Action, Protocol, Source/Dest, Options).

<b>Traffic Analysis:</b> Analyzing packet payloads and link-layer data using Snort's sniffer mode (-dev flags).

<b>Service Identification:</b> Identifying targeted ports (Port 22/SSH) through log correlation.

<b>Linux Security Administration:</b> Operating Snort with elevated privileges and managing log directories.

 </p> </br>

<h2>Investigation Walktrough</h2>
<p>
  <h3>Identifying the Attack Source</h3>
  
I initiated Snort in sniffer and packet logger mode to capture the live attack traffic.

<img src="https://i.imgur.com/K71ClSM.png" height="80%" width="80%" alt="Investigation Walkthrough"/>

Observation: The console showed a high volume of traffic targeting a specific port.

Findings: By filtering the logs for "22", I identified multiple failed SSH connection attempts.

<img src="https://i.imgur.com/OsMp31y.png" height="80%" width="80%" alt="Investigation Walkthrough"/>

<h3>Crafting the IPS Rule</h3>

Once the service (SSH) and attacker IP were confirmed, I formulated a Snort rule to block the traffic.

The Logic: The rule was designed to "drop" any TCP traffic from the attacker's IP to the server's SSH port.

<img src="https://i.imgur.com/DlxLvri.png" height="80%" width="80%" alt="Investigation Walkthrough"/>

<img src="https://i.imgur.com/ZaiIDJ5.png" height="80%" width="80%" alt="Investigation Walkthrough"/>

