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
Establishing a Log Analytics Workspace, which will serve as the repository for the logs. This specialized workspace becomes the central hub where all collected logs, including the custom log with geographic information, will be stored. Subsequently, our Azure Sentinel (SIEM) system will establish a connection to this workspace. This integration enables the seamless display of geospatial data on the map, enhancing our cybersecurity visualization capabilities. Let start by selecting the resource group we created earlier (HoneyNet):  <br/>
<img src="https://i.imgur.com/VYsAP4u.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Next, Type in a name for your Instance Details. I chose "Log-Analytics-Workspace-HoneyNet":  <br/>
<img src="https://i.imgur.com/pujBvWx.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Click Review + create :  <br/>
<img src="https://i.imgur.com/AkK002S.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Then click create :  <br/>
<img src=" https://i.imgur.com/RseDIng.png " height="80%" width="80%">
<br />
<br />
Next, Search for Microsoft Defender for Cloud : <br/>
<img src="https://i.imgur.com/N1wJdMp.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
In this step, we activate the capability to collect logs from the virtual machine and channel them into the Log Analytics Workspace. Scoll down and at the bottom left below "Management" click on "Enviorment Settings" :  <br/>
<img src="https://i.imgur.com/iAZrmb9.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Click on your previously created L.A.W:  <br/>
<img src="https://i.imgur.com/1pYriNi.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Turn on CSPM and Servers, leave SQL OFF:  <br/>
<img src="https://i.imgur.com/KolEvUv.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Next go to Data Collection and click on "ALL EVENTS" then click "Save". :  <br/>
<img src="https://i.imgur.com/2OPUdT0.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Let's go back to our L.A.W :  <br/>
<img src="https://i.imgur.com/wiW7ky0.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Here we're going to connect our L.A.W to the virtual machine. On the left under "classic" you will see "Virtual Machines":  <br/>
<img src="https://i.imgur.com/0Uosmp2.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Open your HoneyNet-vm :  <br/>
<img src="https://i.imgur.com/fWqDyNL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Connect VM :  <br/>
<img src="https://i.imgur.com/AzsV51F.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Let's Move to Azure Sentinel by using our search bar, It will be listed below "Marketplace":  <br/>
<img src="https://i.imgur.com/WyrCPpu.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Connect Workspace :  <br/>
<img src="https://i.imgur.com/Ae41BuO.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Clink on  "Log-Analytics-Workspace-HoneyNet" and add.:  <br/>
<img src="https://i.imgur.com/ks2mPCM.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Let Connect to our VM! In your portal go back to your vm and copy the vm IP  :  <br/>
<img src="https://i.imgur.com/q4g8EQt.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Here we connect our actual pc to the vm. On your windows pull up Remote Desktop Connection.:  <br/>
<img src="https://i.imgur.com/7KPYiQX.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Paste address and coonect:  <br/>
<img src="https://i.imgur.com/ky5HZS3.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Enter your credentials:  <br/>
<img src="https://i.imgur.com/yL58G1v.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Click yes:  <br/>
<img src="https://i.imgur.com/kKZGsJz.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Click no on everything:  <br/>
<img src="https://i.imgur.com/JD1EfUC.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Click Yes:  <br/>
<img src="https://i.imgur.com/sxHEEsg.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Once in you VM open Even viewer:  <br/>
<img src="https://i.imgur.com/Y7QLTsk.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Click on security:  <br/>
<img src="https://i.imgur.com/6AykbsB.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Here is where all your logs are kept:  <br/>
<img src="https://i.imgur.com/sHBqROu.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Just to test it out lets log off and try to log with the wrong password and see what we get:  <br/>
<img src="https://i.imgur.com/5Q626Pb.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Wrong password:  <br/>
<img src="https://i.imgur.com/HDjZMZb.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Failed log in:  <br/>
<img src="https://i.imgur.com/FQSR6FC.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Lets log in and check our logs for a failed log in:  <br/>
<img src="https://i.imgur.com/OzP9012.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Our logs will show a failed log and the failure reason :  <br/>
<img src="https://i.imgur.com/3QWkgMc.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
It will also the IP address of the source (in this case since it was me it shows my IP):  <br/>
<img src="https://i.imgur.com/6vHoF5e.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Lets head over to our free IP Geolocation and get our free API :  <br/>
<img src="https://i.imgur.com/qlRBglV.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Once you have your api keys copied on a note pad head over to the vm and pull up " wf.msc" to turn off of our firewall:  <br/>
<img src="https://i.imgur.com/Yb8rdje.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Should look like this:  <br/>
<img src="https://i.imgur.com/aqv67HK.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Click Windows Defender :  <br/>
<img src="https://i.imgur.com/eM0xjWy.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Turn off Domain Profile:  <br/>
<img src="https://i.imgur.com/kBrbk2n.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Turn off Private Profile:  <br/>
<img src="https://i.imgur.com/NxN1u2K.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Turn off Public Profile:  <br/>
<img src="https://i.imgur.com/ORHkQu2.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Let test out our Connection to the vm. On your actual pc pull up command prompt and type in  "ping [your vm IP] -t " if every done right you command line will look like this:  <br/>
<img src="https://i.imgur.com/gGc1SHG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Lets head back to our vm pull up Powershell ISE:  <br/>
<img src="https://i.imgur.com/gt7F5pn.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Copy the Custom_Security_Log_Exporter.ps1 that listed with this project and paste it here (Make sure you fill in your API KEYS before next step! ). :  <br/>
<img src="https://i.imgur.com/mxUMu1x.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Save this script on your VM desktop:  <br/>
<img src="https://i.imgur.com/fzPgKxe.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Copy the 5th line on the script. This where the Log files will output to this folder.  :  <br/>
<img src="https://i.imgur.com/ufO5zgX.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Pull up Run on your VM. :  <br/>
<img src="https://i.imgur.com/s8ec1fF.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Paste the folder location here and click ok :  <br/>
<img src="https://i.imgur.com/Swnif0J.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Go back to powershell and run script :  <br/>
<img src="https://i.imgur.com/76OU7Mw.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Once you run the script your powershell should look like this with the incoming logs listed on the bottom:  <br/>
<img src="https://i.imgur.com/7VkqERO.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
When you go back to the program data folder you'll see that the incoming logs are store here under " failed_rdp":  <br/>
<img src="https://i.imgur.com/xuOX0Bb.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Open failed_rdp file and copy these first few lines on a note pad:  <br/>
<img src="https://i.imgur.com/Ko0yFTM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Save as Failed_rdp_1:  <br/>
<img src="https://i.imgur.com/iuyQgKV.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Lets go back to our portal and head over to our L.A.W and create a custom log. On the left click tables :  <br/>
<img src="https://i.imgur.com/mjg3fgt.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Create / New custome log (MMA_ BASED):  <br/>
<img src="https://i.imgur.com/wSSDOIp.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Click on the blue folder and select the file that we saved earlier as " Failed_rdp_1". :  <br/>
<img src="https://i.imgur.com/yHqNL47.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Go back to you VM and copy the path where the "Program data" is collected:  <br/>
<img src="https://i.imgur.com/U8B8WV0.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Copy path:  <br/>
<img src="https://i.imgur.com/TA8GEQd.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Go back to azure and under type select  :  <br/>
<img src="https://i.imgur.com/kN4i9sJ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Then paste path:  <br/>
<img src="https://i.imgur.com/uXbH2f9.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Give your custom log a name:  <br/>
<img src="https://i.imgur.com/WRgui0B.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Once that is created it will take a while for L.A.W and the VM to sync. Once it does sync you will be able to see the logs at the bottom when you type the name of your custom log and click run.  :  <br/>
<img src="https://i.imgur.com/BAG3OqD.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Copy the Data extraction script that is post on this project and paste it here and click run:  <br/>
<img src="https://i.imgur.com/B3spPLC.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
You will notice that now our columns labeled with with state, country, time, sourcehost, etc.:  <br/>
<img src="https://i.imgur.com/RL001Jp.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Lets head over to Microsoft Sentinel:  <br/>
<img src="https://i.imgur.com/eor2EGL.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Add Workbook:  <br/>
<img src="https://i.imgur.com/DMXAlMB.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Lets edit our analytics:  <br/>
<img src="https://i.imgur.com/SjVOTFv.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Click the three dots and remove top and bottom:  <br/>
<img src=" https://i.imgur.com/IHqVo32.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Select add:  <br/>
<img src="https://i.imgur.com/1wMERax.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Select add query:  <br/>
<img src="https://i.imgur.com/tP8l7GR.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
open a new tab in azure and go back to you custom log and copy the script:  <br/>
<img src=" https://i.imgur.com/n139b34.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Run query and select map under visualization:  <br/>
<img src="https://i.imgur.com/6DEOX7n.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Change the size of your map to full:  <br/>
<img src="https://i.imgur.com/c7gdS3g.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Save :  <br/>
<img src="https://i.imgur.com/U1vlfmk.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Select you resource group:  <br/>
<img src="https://i.imgur.com/Z62bA6F.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Make sure your location is set to (US) West US 3:  <br/>
<img src="https://i.imgur.com/ZQoq5aX.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Apply :  <br/>
<img src="https://i.imgur.com/XLKMnug.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
WE ARE DONE! Now we can see on a live map were the attacks are coming from.:  <br/>
<img src="https://i.imgur.com/qasRUb7.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Results after 2hrs:  <br/>
<img src="https://i.imgur.com/0J6aeTd.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
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
