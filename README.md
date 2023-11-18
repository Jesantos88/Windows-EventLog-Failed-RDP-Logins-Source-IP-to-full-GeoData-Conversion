<h1>Windows-EventLog-Failed-RDP-Logins-Source-IP-to-full-GeoData-Conversion</h1>

<h2>Description</h2>
In the course of this project, I orchestrated the configuration of Azure Sentinel (SIEM) and established its connection to a live virtual machine, functioning as a honeynet. Within this framework, I demonstrated the step-by-step process of setting up and monitoring real-time attacks, particularly focusing on RDP Brute Force attempts originating from various global locations. Additionally, as part of the project, I employed a customized PowerShell script to extract and cross-reference geolocation information of attackers, dynamically visualizing their locations on the Azure Sentinel Map. This initiative not only showcased the capabilities of Azure Sentinel but also highlighted the real-world applicability of leveraging geospatial insights for enhanced cybersecurity monitoring.
<br />


<h2>Languages and Utilities Used</h2>

- <b>PowerShell</b> 
- <b>ipgeolocation.io: IP Address to Geolocation API </b> 

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
Create New resource Group (A resource group functions as a logical container in Azure, unifying resources that typically have a shared lifespan. For this project, the intent is to organize all components within the same resource group, facilitating streamlined management and ensuring cohesive coordination of related resources. This approach simplifies administration and enhances the overall efficiency of resource allocation and monitoring within the Azure environment.): <br/>
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
Select Image as " Windos 10 PRO" :  <br/>
<img src="https://i.imgur.com/MjOhvaF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Make sure size is Standard_B1s:  <br/>
<img src="https://i.imgur.com/FJzKpPn.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
All Fields should like the following:  <br/>
<img src="https://i.imgur.com/6CMYxkl.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Click next twice for Network interface :  <br/>
<img src="https://i.imgur.com/XsdJpzt.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Select "Advanced" for Nic Network Security Group:  <br/>
<img src="https://i.imgur.com/2cmWxSd.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Create a new network Security Group:  <br/>
<img src="https://i.imgur.com/ycg1IzX.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Remove default Inbound Rules using the trash can on the right of the screen :  <br/>
<img src="https://i.imgur.com/pnzTPF3.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Click +Add an new inbound rule:  <br/>
<img src="https://i.imgur.com/GWronYE.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Change Destination Port from 8080 to a star * for anything:  <br/>
<img src="https://i.imgur.com/xpilqiU.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Change Priority from 1010 to 100:  <br/>
<img src="https://i.imgur.com/kiJWjGn.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Here Name choice can be anything so I chose DANGER_ALERT:  <br/>
<img src="https://i.imgur.com/LIUo0lN.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Click ok :  <br/>
<img src="https://i.imgur.com/2xFPr2l.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Next click Review + create :  <br/>
<img src="https://i.imgur.com/3U8ymGU.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Next Click Create :  <br/>
<img src="https://i.imgur.com/nJ7v64j.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
And Done! Keep in mind that he objective is to intentionally enhance the discoverability of the virtual machine by allowing various means of probing, including TCP pings, SYN scans, and ICMP pings. The intention is to refrain from dropping any traffic, making the virtual machine easily detectable. While this approach is unconventional in typical security practices, it is pursued for specific lab requirements, aiming to simulate scenarios where the system is intentionally exposed and open to discovery and subsequent simulated attacks for educational or testing purposes. 
 (Fyi: Azure will alert once the process is done):<br/>
<img src="https://i.imgur.com/N2Vbnjn.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Head to Log Analytics Workspace by typing it in the search bar:  <br/>
<img src="https://i.imgur.com/SeKycXx.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Create a Log Analytics Workspace. 
The primary objective here is to assimilate logs from the virtual machine. This entails the ingestion of Windows event logs, and additionally, we will generate a bespoke log incorporating geographic information. This strategic approach enables us to discern the origins of potential attackers and enhance our understanding of their geographical locations.:  <br/>
<img src="https://i.imgur.com/c6GfRev.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
:  <br/>
<img src=" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
:  <br/>
<img src=" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
:  <br/>
<img src=" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
:  <br/>
<img src=" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
:  <br/>
<img src=" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
:  <br/>
<img src=" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
:  <br/>
<img src=" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
:  <br/>
<img src=" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
:  <br/>
<img src=" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
:  <br/>
<img src=" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
:  <br/>
<img src=" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
:  <br/>
<img src=" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
:  <br/>
<img src=" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
:  <br/>
<img src=" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
:  <br/>
<img src=" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
:  <br/>
<img src=" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
:  <br/>
<img src=" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
:  <br/>
<img src=" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
:  <br/>
<img src=" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
:  <br/>
<img src=" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
:  <br/>
<img src=" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
:  <br/>
<img src=" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
:  <br/>
<img src=" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
:  <br/>
<img src=" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
:  <br/>
<img src=" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
:  <br/>
<img src=" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
:  <br/>
<img src=" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
:  <br/>
<img src=" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
:  <br/>
<img src=" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
:  <br/>
<img src=" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
:  <br/>
<img src=" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
:  <br/>
<img src=" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
:  <br/>
<img src=" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
:  <br/>
<img src=" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
:  <br/>
<img src=" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />








































































































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
