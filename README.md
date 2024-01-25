<h1> In-Depth Analysis of a Suspicious Registry File</h1>

<h2>Brief Description</h2>
<p> This project focuses on investigating an unexpected file discovered in the registry of an employee's computer by an IT technician. The primary objective is to determine whether this file is legitimate or malicious using various log sources in Splunk and Open Source Intelligence (OSINT). </p> 

<p> The investigation involves identifying the Windows feature associated with the file (osk.exe), its expected file path, and analyzing Sysmon events to determine the number of related events and the full file path of the suspicious executable. </p>

<p> Furthermore, by investigating osk.exe communication and hash properties; we will use VirusTotal to examine malware function and behavior to piece together a complete picture of the file's nature/activities. </p>
  
<h2>Utilities Used</h2>
<ul>
    <li><b>Splunk</b></li>
    <li><b>OSINT</b></li>
    <li><b>VirusTotal</b></li>
</ul>
    
<h2>Project Walk-Through</h2>

<h3> Perform OSINT research on OSK.exe & expected file path</h3>
<p> C:\Windows\System32 </p>
<img src="https://imgur.com/6Do470k.png" height="80%" width="80%" alt="FTK Imager Memory Capture">
<img src="https://imgur.com/a4qIZSo.png" height="80%" width="80%" alt="FTK Imager Memory Capture">

<h3> Analyze Sysmon event of osk.exe and determine file path </h3>
<p> Command => <code>index="botsv1" sourcetype="xmlwineventlog" osk.exe</code> </p>
<img src="https://imgur.com/RbRY3yn.png" height="80%" width="80%" alt="FTK Imager Memory Capture">

<h3>Identify Workstation </h3> 
<p> System details where the file is operational, including the computer name, IP address, and user account </p>
<img src="https://imgur.com/vmM2F3C.png" height="80%" width="80%" alt="FTK Imager Memory Capture">
<p> Directory => C:\Users\bob.smith.WAYNECORPINC\AppData\Roaming\{35ACA89F-933F-6A5D-2776-A3589FB99832}\osk.exe </p>
<p> Computer => we8105desk.waynecorpinc.local </p>
<p> SourceIp => 192.168.250.10 </p>
<p> User => Bob.smith </p>

<h3>Investigate network communications </h3>
<p> Analyzing destination ports and unique destination IP addresses connected to the file.</p>
<p> Scroll to Destination Port in Interesting Fields </p>
<img src="https://imgur.com/j8sh8b2.png" height="80%" width="80%" alt="FTK Imager Memory Capture">
<p> Destination port 6892 has the highest volume of traffic</p>
<p> Adding destination port into the query and identifying the number of unique destination IP addresses the file is connecting to.</p>
<p> Command => <code> index="botsv1" sourcetype="xmlwineventlog" osk.exe Image="C:\\Users\\bob.smith.WAYNECORPINC\\AppData \\Roaming\\{35ACA89F-933F-6A5D-2776-A3589FB99832}\\osk.exe" DestinationPort=6892 |  stats count by DestinationIp </code></p>
<img src="https://imgur.com/m1nrNER.png" height="80%" width="80%" alt="FTK Imager Memory Capture">

<h3> Retrieve SHA256 Hash for external analysis</h3>
<p> Query Sysmon Event ID 7 with the image file. Command => <code> index="botsv1" sourcetype="xmlwineventlog" ImageLoaded=*osk.exe EventID=7 </code> </p>
<img src="https://imgur.com/zWfCI8w.png" height="80%" width="80%" alt="FTK Imager Memory Capture">
<p> Input Sha256 hash into VirusTotal for file reputation analysis</p>
<img src="https://imgur.com/12yCIu7.png" height="80%" width="80%" alt="FTK Imager Memory Capture">
<p> Potential Malware name: Cerber Malware </p>

<h3>Use FortiGate Unified Threat Management logs for complementary analysis </h3>
<p> Retrieve application category, action, critical level, etc using command => <code>index="botsv1" sourcetype="fortigate_utm" dest_port=6892</code> </p>
<img src="https://imgur.com/12yCIu7.png" height="80%" width="80%" alt="FTK Imager Memory Capture">

















