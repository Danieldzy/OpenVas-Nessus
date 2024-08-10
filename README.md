<h1>Vulnerability Scan</h1>

<h2>Considerations</h2>

Conducting a vulnerability scan can consume significant resources on both your host and target systems, potentially causing system downtime if not managed properly. As a Cybersecurity professional, it is crucial to understand your environment thoroughly before initiating such scans, adhering to GRC standards. Ensure clear communication with your manager and schedule scans for off-peak hours to reduce production impact. Begin by testing in a staging environment, and if you plan to automate scans, first conduct manual tests in a smaller production environment to verify that everything runs smoothly. It is crucial that all stakeholders are informed about the scan and understand the potential impact.

In this 

<h2>Utilities Used</h2>

- <b>Tenable Nessus</b>
- <b>Terminal</b> 
- <b>Greenbone OpenVas</b>

<h2>Environments Used </h2>

- <b>Windows 11</b> 
- <b>Kali Linux</b> 
- <b>Ubuntu Linux</b>

<h2>Part 1</h2>
The project involves two virtual machines: Windows 11 (10.0.0.61) and Metasploitable (with the IP address 10.0.0.188) with a bridge network. An unauthenticated scan was executed using Tenable Nessus from Windows 11, targeting the Metasploitable VM, and a report was generated to identify vulnerabilities. Note: Part 2 is OpenVas Scan on the same target. Unauthenticated scans were performed by both OpenVAS and Tenable Nessus, with the OpenVAS scan covering more ports than the Tenable Nessus scan. A brief comparison of the reports will be conducted.

<h2>Program walk-through:</h2>

<p align="center">
Diagram 1: <br/>
<br/><img src="https://imgur.com/IGxcgKh.png" height="50%" width="50%" alt=""/><br/>


<br/>First, ping the Metasploitable machine from your Windows 11 system to ensure that both machines are connected via the bridged network.<br/>
<br/><img src="https://imgur.com/TQfFBTk.png" height="80%" width="80%" alt=""/><br/>

<br/>Download Tenable Nessus, and after the download, verify the hash value of the file. This step is essential to confirm that the file has not been tampered with and maintains its integrity.<br/>
<br/> Run command: Get-FileHash .\filename.msi on PowerShell or CLI to check the hash value and compare it to the value provided on the Tenable Nessus downloading page.<br/> 
<br/><img src="https://imgur.com/pgySYFp.png" height="80%" width="80%" alt=""/><br/>


<br/>Install Nessus and connect via SSH, Select Show Advanced and Continue to Localhost, you can see from the URL that Nessus runs on Port 8843. Be sure to install the Nessus Essentials (free version), which limits you to scanning fewer than 16 IP addresses at a time. For this lab demonstration, that limitation is sufficient. The Essentials version is robust enough to perform complex scans and deliver accurate reports. You will need an activation key for Nessus, register using your email to get the key.<br/>
<br/><img src="https://imgur.com/3qCWYdD.png" height="80%" width="80%" alt=""/><br/>

<br/>When you log into Nessus for the first time, the plugins will automatically compile, which you can observe in the top right corner. As information about new vulnerabilities is discovered and released into the general public domain, Tenable, Inc. research staff designs programs to enable Tenable Nessus to detect them. These programs are called plugins. Plugins contain vulnerability information, a generic set of remediation actions, and the algorithm to test for the presence of the security issue. Therefore please patiently wait for the compiling plugin process to finish (20-40 minutes), Please note that to include a specific plugin in a scan, you must choose the "Advanced Scan" option. The "Basic Scan" does not support adding custom plugins.<br/>
<br/><img src="https://imgur.com/LJmIQbF.png" height="80%" width="80%" alt=""/><br/>

<br/>Select New Scan, Choose The Basic Network Scan template, it is ideal for initial assessments and provides a good overview of potential vulnerabilities with a relatively simple configuration. You can also choose other options for other specific needs:<br/>
<br/><img src="https://imgur.com/Jyjz52D.png" height="80%" width="80%" alt=""/><br/>

<br/>In Settings, under General, Add the name and target IP for the scan. In the Credential section, add the target(Metasploitable) credentials: msfadmin for both username and password, and choose Authentication method as password. This will conduct an authenticated scan, which provides a more detailed and thorough assessment, potentially identifying more vulnerabilities. However, it may also result in a higher number of false positives.<br/>
<br/><img src="https://imgur.com/l2QvHbf.png" height="80%" width="80%" alt=""/><br/>
<br/><img src="https://imgur.com/7syElmX.png" height="80%" width="80%" alt=""/><br/>

<br/>In Settings, under Discovery, you can select either a common ports scan or an all ports scan. Under Assessment, you can choose between a Quick scan or a Complex scan. For this demonstration, I will select the common ports and Quick scan WITHOUT credentials. This choice allows me to compare the scan results with those from Part 2 of this project, where OpenVAS is used on the same target. By analyzing these results, I will provide a comparison of both Nessus and OpenVAS as vulnerability scanning tools.<br/>
<br/><img src="https://imgur.com/3EwFzlL.png" height="80%" width="80%" alt=""/><br/>

<br/>Click "Launch" to start your first scan. The duration of the scan will vary based on your settings. Once the scan is complete, click on the "My Scans" folder on the left and select your scan name to view the vulnerabilities. You can generate and download the report from there.<br/>
<br/><img src="https://imgur.com/0QJuIxt.png" height="80%" width="80%" alt=""/><br/>

<br/><br/>

<h2>Part 2</h2>
The project involves two virtual machines: Kali Linux (with the IP address 10.0.2.8) and Metasploitable (with the IP address 10.0.2.15). An authenticated scan was executed using OpenVAS from the Kali Linux VM, targeting the Metasploitable VM, and a report was generated to identify vulnerabilities. Note: the final report is an Unauthenticated Scan report due to Kali blocking the target's SSH port 22, even though the correct credential has been provided. A detailed explanation can be found at the end.
<br />

<h2>Program walk-through:</h2>

<p align="center">
Diagram 2: <br/>
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
For new target, go to the Configuration tab, select Targets, click on the Add New Target icon at the top left of the screen, name the target, I name it as a 'Cred' since it is an authenticated scan. Manually type in the IP address. In Credentials for authenticated checks, select SSH since Metasploiable is based on ubuntu Linux, and select the Credential you just created. For Port Lists, I choose the Nmap option instead of IANA because it scans more ports, making it better suited for an intense scan.
<br />
<img src="https://imgur.com/ayTm11U.png" height="80%" width="80%" alt="Add New Target"/>
<br />


You can initiate your scan by going to the Scans tab and selecting New Task. Name your scan and choose the target you created (MetasploitableVM Cred in my case). The minimum QoD is set to 70% by default to reduce false positives, but you can adjust this value if needed. Increasing the QoD setting generally improves the accuracy of the scan and reduces the likelihood of false positives, it may also reduce the number of vulnerabilities detected. For Scan Config, choose Full and Fast, it combines a comprehensive scan with optimizations for speed. Other Scan Config options you can find more detail here: https://docs.greenbone.net/GSM-Manual/gos-21.04/en/scanning.html#scanconfigs
<br/>
<img src="https://imgur.com/V4m6fWx.png" height="80%" width="80%" alt="Start Scan"/>

<br />
After saving the scan task, click the triangle start button below to initiate the scan (It may take a longer time depending on your Kali resources). Once the scan is complete, you can download the scan report from the Scans tab by selecting Reports, clicking on the report name, and then clicking the Download Filtered Report icon. Please find the uploaded report in this repository, The results include the CVSS score, mitigation solutions, affected software/OS, CVE references, and other valuable details.
<br />
<img src="https://imgur.com/GOGSJpj.png" height="80%" width="80%" alt=""/>
<br />
Note on the report it says "10.0.2.15 - METASPLOITABLE SSH Failure Protocol SSH, Port 22, User msfadmin : Login failure". This means that Kali Linux is blocking the SSH port 22 on Metasploitable due to outdated encryption methods. If you try to connect the Metasploitable using "ssh msfadmin@10.0.2.15" command, the error message "Unable to negotiate with 10.0.2.15 port 22: no matching host key type found. Their offer: ssh-rsa,ssh-dss" indicates a mismatch in keys and outdated encryption, specifically ssh-rsa and ssh-dss, which are considered insecure. This results in an Unauthenticated Scan Report, even though a correct credential was provided. Despite this, the scan still identified 69 vulnerabilities, with 24 of them rated high in CVSS scores.
<br />




