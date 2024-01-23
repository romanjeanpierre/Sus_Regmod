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
<p> Command => index="botsv1" sourcetype="xmlwineventlog" osk.exe </p>
<img src="https://imgur.com/RbRY3yn.png" height="80%" width="80%" alt="FTK Imager Memory Capture">

<h3>  TBD </h3>


