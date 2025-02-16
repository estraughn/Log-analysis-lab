# Log analysis lab

## Objective
To develop expertise in analyzing and interpreting system and security logs to identify anomalies, detect threats, and enhance incident response and forensic investigation capabilities.



### Skills Learned

- Gathered logs from diverse sources, such as servers, firewalls, applications, and endpoints, for centralized analysis
- Used Splunk to analyze and visualize log data for threat detection
- Identifyed patterns, anomalies, and trends in log data to uncover potential security incidents or system issues
- Detected indicators of compromise (IOCs) and other malicious activities within log data
- Set up alerts and dashboards for continuous log monitoring to quickly identify critical events
- Used logs to diagnose and resolve system errors, misconfigurations, or vulnerabilities



### Tools Used

- Splunk
- Splunk BOTS
- Virus Total
- Cisco Talos
- Cyber Kill Chain
- Mitre Att&ck Framework 

## Steps
1. To start I utilized the Splunk Boss of the SOC website to complete their website defacement CTF challenge. I first analyzed the breifing of the sample scenario and then proceeded to log onto the Splunk cloud instance to begin my investigation.

*Ref 1.1:![Screenshot 2025-02-16 140449](https://github.com/user-attachments/assets/40de2b7e-e525-4fd4-87f6-18841cd8fb37)

*Ref 1.2:![Screenshot 2025-02-16 140738](https://github.com/user-attachments/assets/18ffb3f7-00e8-4208-a29e-6efbd0fe97fd)

2. Based on the information proivded about the event I know the time frame was between 08/16/2016 - 08/31/2016 so I narrowed my search criteria time frame to this range. Then using SPL I searched the provided index for firewall logs to begin compiling the IOC. I further narrowed my search based on the fact the attacker used the nessus vulnerability scanner tool to perform reconissance which means the firewall would have blocked some of the packets. So I added action=blocked to my SPL request and took note of the fact that there was only one ip address in the source ip's that had blocked network traffic. Next I did a search for http traffic from our host endpoint to and from the suspect ip address to uncover more information. Next I searched the suricata logs for events related to "imreallynotbatman.com" and filtered it down to the top 10 results. 


*Ref 2.1:![Screenshot 2025-02-16 142240](https://github.com/user-attachments/assets/45ad2b11-2d36-4147-a594-2f72e19a4cba)

*Ref 2.2:![Screenshot 2025-02-16 142843](https://github.com/user-attachments/assets/0cf0bb41-0a7e-4e83-99e8-c31fe389472a)

*Ref 2.3:![Screenshot 2025-02-16 143805](https://github.com/user-attachments/assets/cc893372-e1ad-4935-b3f1-1666a91b0a2c)

*Ref 2.4:![Screenshot 2025-02-16 144759](https://github.com/user-attachments/assets/7aa13c11-b449-459c-9f5d-e9c20b63b56b)

3. Then based on the IOC I collected in my initial investigation was able to run a query to establish how the attacker attempted to expolit a vulnerability. Which the results from my search indicated that the attacker attempted a brute force attack to gain access to the adminstrator account. Next I made a query to uncvoer the payload that the attacker weaponized by searching for executable files connected to the "imreallynotbatman.com" source type. It was noted that two executable files were uploaded to the server 3791.exe and /_vti_bin/shtml.exe. With that documented I chose to perform a further investigation on the 3791.exe file based on the fact that it came from our previously documented IOC ip address. I then filtered down the firewall logs and noted the event information in Ref 3.4. I then took the files hash value and went over to Virus Total to find out if this is a known malicious hash value. My next step was to find the name of the file that defaced the "imreallynotbatman.com" website which I did by creating the query shown in Ref 3.6. The results of this query provided me with the file name within the results of the event. 

*Ref 3.1: ![Screenshot 2025-02-16 150243](https://github.com/user-attachments/assets/2dde1c62-369c-4247-9dc3-6c813e300dfd)

*Ref 3.2:![Screenshot 2025-02-16 150752](https://github.com/user-attachments/assets/49fbcb08-d58d-42c7-a865-72d646818f55)

*Ref 3.3:![Screenshot 2025-02-16 150809](https://github.com/user-attachments/assets/561e67c8-2b12-4957-aee7-c0f4aa4efa74)

*Ref 3.4:![Screenshot 2025-02-16 151752](https://github.com/user-attachments/assets/fb7eb8aa-d5e0-4112-b37c-579f12681c57)

*Ref 3.5:![Screenshot 2025-02-16 152122](https://github.com/user-attachments/assets/9ac1b90e-5305-472a-9be5-d4f9cadac847)

*Ref 3.6:![Screenshot 2025-02-16 153243](https://github.com/user-attachments/assets/a733774e-2e7c-4409-8d0c-7281f18aa54d)

*Ref 3.7:![Screenshot 2025-02-16 153407](https://github.com/user-attachments/assets/a7f1239a-dfca-4660-94b1-295073b45cca)

4. Now it was time to investigate the malicious domain hosting the ip address of the attacker. from my previously run query I was able to see the domain "jumpingcrab.com" I took note of this and did a search on Cisco Talos to find out more information. I then went to the Mitre Att&ck framework to map the attackers TTP's to try and understand these threats better for overall SOC improvement. 

*Ref 4.1:![Screenshot 2025-02-16 154719](https://github.com/user-attachments/assets/085d28ac-d360-460c-89f1-b57327a93047)

*Ref 4.2:![Screenshot 2025-02-16 155116](https://github.com/user-attachments/assets/a2e9a6bb-8f9c-429e-9ae9-91805397a0a3)

*Ref 4.3:![Screenshot 2025-02-16 162136](https://github.com/user-attachments/assets/568ecfd2-e4e4-4535-a181-b8408959db75)

5. Now that the log analysis is complete here are a few proactive steps that can be taken to improve the organizations security posture in the future:

- Update firewall and IDS/IPS rules based on attack patterns observed in the logs
- Enhance endpoint security policies (e.g., blocking malicious IPs, restricting unnecessary ports)
- Implement stricter authentication controls like MFA or privileged access management (PAM)
- Share new IOCs (Indicators of Compromise) with threat intelligence platforms (MISP, AlienVault, etc.)
- Implement or update SOAR (Security Orchestration, Automation, and Response) for faster response
- Automate threat hunting playbooks for specific attack vectors found in log analysis
- Patch and secure system vulnerablilites













