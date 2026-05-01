# C2-Beaconing-Wireshark
# Detecting Command-and-Control (C2) Beaconing Using Wireshark

## 1. Project Overview

This project demonstrates how Wireshark can be used to detect and analyze network traffic indicative of command-and-control (C2) beaconing. (C2) beaconing is a technique used by malware to communicate with an external server at regular intervals to maintain control, recieve commands, or exfiltrate data while evading detection. The objective is to simulate realistic attacker communication using HTTP and identify suspicious behavioral patterns through packet analysis.

A controlled environment was created where a host repeatedly communicated with an external system over port 8080. Using Wireshark, this traffic was captured, filtered, and analyzed to identify indicators of compromise such as periodic communication, repeated sessions, and application-layer activity. This project provides insight into how SOC and IR teams support detection and investigation. 

---

## 2. Project Relevance
_ Perry


## 3. Methodology
-Yoelbis/Cara

### 3.1 Environment Setup

- Virtualized lab environment (VirtualBox)
- Internal network range: `192.168.56.0/24`
  
### Victim Machine (Ubuntu): Simulated Compromised Host 
- IP Address:`192.168.56.103`
- Confirm that victim can reach attacker machine via ping, this verifies network is working before traffic generation
```bash
ping 192.168.56.101
```
- Script:
```bash
nano beacon.py
```
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

## 4. Incident Response Framework (NIST SP 800-61)
- This project follows the NIST SP 800-61 Computer Security Incident Handling Guide to structure the detection and analysis of Command-and-Control (C2) beaconing activity. This framework provides a systematic approach to identifying, analyzing, and responding to security incidents.
### 4.1 Preparation 
- A controlled lab environment was created using two virtual machines to simulate a real-world attack scenario. As you can see earlier we have two different Virtual Machines set up.
- Wireshark was configured to capture and monitor traffic between both machines. This setup allowed for safe testing and observation of malicious communication patterns.

### 4.2 Dectection & Analysis (Primary Focus) 
- This phase represents the core of the project, where Wireshark was used to analyze network traffic and detect C2 beaconing behavior.
- Captured and inspected network traffic using Wireshark
- Applied filters to isolate relevant communication:
- ```bash
  http && tcp.port == 8080
  http.request && ip.src == 192.168.56.103
  ip.addr == 192.168.56.103 && tcp.poty == 8080
  ```

## 5. Results 

- Present your findings using graphs, screenshots, logs, or tables.
- Show evidence that supports your conclusions.
- I/O Graph
- Screenshots
- GET/HTTP


## 6. Conclusion:
- Summarize key insights about IR, lessons learned, and/or
- potential improvements
### 6.1 Post Incident Activity
- Documented all findings and observed Indicators of Compromise (IoCs)
- Analyzed traffic patterns to understand attacker behavior
- Recommended improved monitoring and detection strategies
  
### 6.2 Containment, Eradication, and Recovery
Although this project was conducted in a simulated environment, the following actions would be recommended in a real-world scenario: 
- block communication to the attacker IP address 
- terminate suspicious processes on the victim machine
- remove malicious scripts ('beacon.py')
- patch vulnerabilities and secure the system 





