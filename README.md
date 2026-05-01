# Detecting Command-and-Control (C2) Beaconing Using Wireshark

## 1. Project Overview

This project demonstrates how Wireshark can be used to detect and analyze network traffic indicative of command-and-control (C2) beaconing. (C2) beaconing is a technique used by malware to communicate with an external server at regular intervals to maintain control, recieve commands, or exfiltrate data while evading detection. The objective is to simulate realistic attacker communication using HTTP and identify suspicious behavioral patterns through packet analysis.

A controlled environment was created where a host repeatedly communicated with an external system over port 8080. Using Wireshark, this traffic was captured, filtered, and analyzed to identify indicators of compromise such as periodic communication, repeated sessions, and application-layer activity. This project provides insight into how SOC and IR teams support detection and investigation. 

---

## 2. Project Relevance 
Wireshark is a widely used open source network protocol analyzer that plays a critical role in the Detection and Analysis phases of the Incident Response (IR) lifecycle. Wireshark plays a critical role in modern incident response, where the priority is no longer just about identifying malware, but also about understanding attacker behavior. 

#### Why Wireshark is Important in IR: 
- Provides deep packet-level visibility into network communications  
- Enables analysts to validate suspicious activity  
- Helps identify command-and-control behavior, data exfiltration, and anomalies  
- Supports rapid triage during security incidents
  - MTTI (mean time to identify): time between the start of SOC triage and the moment an incident handler confirms that the events and alert(s) constitute an incident

#### When/How Wireshark is Used: 
- Validation after receving initial alerts 
- During network traffic investigations 
- To support timeline reconstruction 

#### Skills Learned: 
- Packet analysis, filtering, custom profile configuration, exporting objects 
- Identifying behavioral indicators of compromise 
- Mapping technical findings to real-world attacker techniques (MITRE ATT&CK)

In this C2 beaconing simulation, Wireshark provided detailed visibility into network activity by allowing us to analyze packet-level communication between systems. It enabled us to observe connection timing, session behavior, and repeated HTTP requests, helping distinguish normal traffic from patterns indicative of automated or controlled communication.

Additionally....need to add perry's stuff 

---
## 3. Methodology

### 3.1 Environment Setup

- Virtualized lab environment (VirtualBox)
- Internal network range: `192.168.56.0/24`

<img width="501" height="321" alt="Environment" src="https://github.com/user-attachments/assets/78d40a27-fd21-462f-8f07-d2ce982b64d9" />
  
### Victim Machine (Ubuntu): Simulated Compromised Host 
- IP Address:`192.168.56.103`
- Confirm that victim can reach attacker machine via ping, this verifies network is working before traffic generation
```bash
ping 192.168.56.101
```
- 🔧Beaconing Simulation Script:
```bash
nano beacon.py
```
See script: [`scripts/beacon.py`](scripts/beacon.py)

This script simulates periodic HTTP requests to mimic command-and-control (C2) beaconing behavior.

- Execute Script:
```bash
python3 beacon.py
```
- Behavior: sends an HTTP GET request from the victim to the attacker server, then sleeps for a randomized interval between 8 and 15 second

### Attacker Machine (Kali): 
- Ip Address: `192.168.56.101`
- Python HTTP server (C2 simulation)
```bash
python3 -m http.server 8080
```
- Behavior: attack machine is now acting as a lightweight command-and-control server listening on TCP port 8080
### Tools Used

- Wireshark (primary analysis tool)
---

## 4. Incident Response Framework (NIST SP 800-61)
- This project follows the NIST SP 800-61 Computer Security Incident Handling Guide to structure the detection and analysis of Command-and-Control (C2) beaconing activity. This framework provides a systematic approach to identifying, analyzing, and responding to security incidents.
### 4.1 Preparation 
- A controlled lab environment was created using two virtual machines to simulate a real-world attack scenario. As you can see earlier we have two different Virtual Machines set up.
- Wireshark was configured to live capture and monitor traffic between both machines. This setup allowed for safe testing and observation of malicious communication patterns.

### 4.2 Dectection & Analysis (Primary Focus) 
- This phase represents the core of the project, where Wireshark was used to analyze network traffic and detect C2 beaconing behavior.
- Captured and inspected network traffic using Wireshark
- Applied filters to isolate relevant communication:
- ```bash
  http && tcp.port == 8080
  http.request && ip.src == 192.168.56.103
  ip.addr == 192.168.56.103 && tcp.poty == 8080
  ```
### 4.3 Findings
- The victim machine generated periodic HTTP GET requests to attacker server
- Communication occurred at consistent intervals (8-15 seconds)
- Traffic consisted of low-volume, repetitive requests typical of beaconing behavior

---
## 5. Results 
- Present your findings using graphs, screenshots, logs, or tables.
- Show evidence that supports your conclusions.
- I/O Graph
- Screenshots
- GET/HTTP
---
## 6. Conclusion:
- Summarize key insights about IR, lessons learned, and/or
- potential improvements
### 6.1 Post Incident Activity
- Documented all findings and observed Indicators of Attack (IoAs)
- Analyzed traffic patterns to understand attacker behavior
- Recommended improved monitoring and detection strategies
- Mature SOCs prioritize IoAs as they help prevent escalation, and then enrich with IoC
  
### 6.2 Containment, Eradication, and Recovery
Although this project was conducted in a simulated environment, the following actions would be recommended in a real-world scenario: 
- Block communication to the attacker IP address 
- Terminate suspicious processes on the victim machine
- Remove malicious scripts ('beacon.py')
- Patch vulnerabilities and secure the system
---
## 7. Acknowledgement/Resources 

National Institute of Standards and Technology. (2012). Computer security incident handling guide (SP 800-61 Rev. 2). U.S. Department of Commerce. https://doi.org/10.6028/NIST.SP.800-61r2

Palo Alto Networks Unit 42. (n.d.). *Customizing Wireshark: Changing column display*.  
https://unit42.paloaltonetworks.com/unit42-customizing-wireshark-changing-column-display/

Palo Alto Networks Unit 42. (n.d.). *Using Wireshark: Display filter expressions*.  
https://unit42.paloaltonetworks.com/using-wireshark-display-filter-expressions/

Palo Alto Networks Unit 42. (n.d.). *Using Wireshark: Identifying hosts and users*.  
https://unit42.paloaltonetworks.com/using-wireshark-identifying-hosts-and-users/

Wireshark Foundation. (n.d.). *Building display filter expressions*.  
https://www.wireshark.org/docs/wsug_html_chunked/ChWorkBuildDisplayFilterSection.html

Wireshark Foundation. (n.d.). Wireshark user guide. https://www.wireshark.org/docs/wsug_html_chunked/​

###  AI Assistance Disclosure
This project was supported by AI-assisted tools for formatting and explanation. All technical analysis and conclusions were independently validated.

​






