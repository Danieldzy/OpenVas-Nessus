<h1>OpenVas scan for vulnerability</h1>

<h2>Considerations</h2>

Conducting a vulnerability scan can consume significant resources on both your host and target systems, potentially causing system downtime if not managed properly. As a Cybersecurity professional, it is crucial to understand your environment thoroughly before initiating such scans, adhering to GRC standards. Ensure clear communication with your manager and schedule scans for off-peak hours to reduce production impact. Begin by testing in a staging environment, and if you plan to automate scans, first conduct manual tests in a smaller production environment to verify that everything runs smoothly. It is crucial that all stakeholders are informed about the scan and understand the potential impact.

<h2>Description</h2>
The project involves two virtual machines: Kali Linux (with the IP address 10.0.2.8) and Metasploitable (with the IP address 10.0.2.15). An authenticated scan was executed using OpenVAS from the Kali Linux VM, targeting the Metasploitable VM, and a report was generated to identify vulnerabilities. Note: the final report is an Unauthenticated Scan report due to Kali blocking the target's SSH port 22, even though the correct credential has been provided. A detailed explanation can be found at the end.
<br />


<h2>Utilities Used</h2>

- <b>Terminal</b> 
- <b>OpenVas</b>

<h2>Environments Used </h2>

- <b>Kali Linux</b> 
- <b>Ubuntu Linux</b> 

<h2>Program walk-through:</h2>



<p align="center">
The Diagram: <br/>
<img src="https://imgur.com/HBmnFE4.png" height="80%" width="80%" alt=""/>



<br/>Run OpenVas on Kali Linux use the command:sudo gvm-start, go to the Administration tab, and check the Feed Status, make sure the feed has been updated, you can use the following command to update feed status in the terminal:

<br/>Update NVT: sudo greenbone-nvt-sync
<br/>Update SCAP: sudo greenbone-scapdata-sync
<br/>Update CERT: sudo greenbone-certdata-sync<br/>
<img src="https://imgur.com/2DNteDn.png" height="80%" width="80%" alt="Add Credential"/>

<br />
First we create new credential. To add a credential for an authenticated scan in GVM, go to the Configuration tab, select Credentials, click on the Add New Credential icon at the top left of the screen, name the credential 'MetasploitableVM'(you can choose other names), and enter the username and password as 'msfadmin'.  <br/>
<img src="https://imgur.com/p9w7qCN.png" height="80%" width="80%" alt="Add Credential"/>
<br />

<br />
Then we create new Host and target. To add a host in GVM, go to the Assets tab, select Hosts, click on the Add New Host icon at the top left of the screen, name the host Metasploitable, and type in its IP address.   <br/>
<img src="https://imgur.com/EI849fV.png" height="80%" width="80%" alt="Add New Host"/>
<br />
For new target, go to the Configuration tab, select Targets, click on the Add New Target icon at the top left of the screen, name the target, I name it as a 'Cred' since it is an authenticated scan. In Credentials for authenticated checks, select SSH since Metasploiable is based on ubuntu Linux, and select the Credential you just created. For Port Lists, I choose the Nmap option instead of IANA because it scans more ports, making it better suited for an intense scan.
<br />
<img src="https://imgur.com/vz3ZUIf.png" height="80%" width="80%" alt="Add New Target"/>
<br />


You can initiate your scan by going to the Scans tab and selecting New Task. Name your scan and choose the target you created (MetasploitableVM Cred in my case). The minimum QoD is set to 70% by default to reduce false positives, but you can adjust this value if needed. Increasing the QoD setting generally improves the accuracy of the scan and reduces the likelihood of false positives, it may also reduce the number of vulnerabilities detected. For Scan Config, choose Full and Fast, it combines a comprehensive scan with optimizations for speed. Other Scan Config options you can find more detail here: https://docs.greenbone.net/GSM-Manual/gos-21.04/en/scanning.html#scanconfigs
<br/>
<img src="https://imgur.com/V4m6fWx.png" height="80%" width="80%" alt="Start Scan"/>

<br />
After saving the scan task, click the triangle start button below to initiate the scan. Once the scan is complete, you can download the scan report from the Scans tab by selecting Reports, clicking on the report name, and then clicking the Download Filtered Report icon. Please find the uploaded report in this repository, The results include the CVSS score, mitigation solutions, affected software/OS, CVE references, and other valuable details.
<br />

<br />
Note on the report it says "10.0.2.15 - METASPLOITABLE SSH Failure Protocol SSH, Port 22, User msfadmin : Login failure". This means that Kali Linux is blocking the SSH port 22 on Metasploitable due to outdated encryption methods. If you try to connect the Metasploitable using "ssh msfadmin@10.0.2.15" command, the error message "Unable to negotiate with 10.0.2.15 port 22: no matching host key type found. Their offer: ssh-rsa,ssh-dss" indicates a mismatch in keys and outdated encryption, specifically ssh-rsa and ssh-dss, which are considered insecure. This results in an Unauthenticated Scan Report, even though a correct credential was provided. Despite this, the scan still identified 69 vulnerabilities, with 24 of them rated high in CVSS scores.
<br />




