<h1>JWipe - Disk Sanitization</h1>

<h2>Description</h2>
In the course of this project, I orchestrated the configuration of Azure Sentinel (SIEM) and established its connection to a live virtual machine, functioning as a honeynet. Within this framework, I demonstrated the step-by-step process of setting up and monitoring real-time attacks, particularly focusing on RDP Brute Force attempts originating from various global locations. Additionally, as part of the project, I employed a customized PowerShell script to extract and cross-reference geolocation information of attackers, dynamically visualizing their locations on the Azure Sentinel Map. This initiative not only showcased the capabilities of Azure Sentinel but also highlighted the real-world applicability of leveraging geospatial insights for enhanced cybersecurity monitoring.
<br />


<h2>Languages and Utilities Used</h2>

- <b>PowerShell</b> 

<h2>Environments Used </h2>

- <b>Windows 10</b> (21H2)

<h2> Project walk-through:</h2>

The primary objective is to assimilate logs from the virtual machine. This entails the ingestion of Windows event logs, and additionally, we will generate a bespoke log incorporating geographic information. This strategic approach enables us to discern the origins of potential attackers and enhance our understanding of their geographical locations.

<p align="center">
First Login to Mirco Soft Azure and go to Virtual Machines: <br/>
<img src="https://i.imgur.com/84FsJw6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Select Create Virtual Machine  :  <br/>
<img src="https://i.imgur.com/jcFmrHg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Create New resource Group: <br/>
<img src="https://i.imgur.com/Gs860Aa.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Name Vm:  <br/>
<img src="https://i.imgur.com/f0BiMIl.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Select region :  <br/>
<img src="https://i.imgur.com/mRqRmyi.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Sanitization complete:  <br/>
<img src="https://i.imgur.com/K71yaM2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Observe the wiped disk:  <br/>
<img src="https://i.imgur.com/AeZkvFQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>














































































































































</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
