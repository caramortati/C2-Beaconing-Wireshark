# C2-Beaconing-Wireshark
# Detecting Command-and-Control (C2) Beaconing Using Wireshark

## 1. Project Overview

This project demonstrates how Wireshark can be used to detect and analyze network traffic indicative of command-and-control (C2) beaconing. (C2) beaconing is a technique used by malware to communicate with an external server at regular intervals to maintain control, recieve commands, or exfiltrate data while evading detection. The objective is to simulate realistic attacker communication using HTTP and identify suspicious behavioral patterns through packet analysis.

A controlled environment was created where a host repeatedly communicated with an external system over port 8080. Using Wireshark, this traffic was captured, filtered, and analyzed to identify indicators of compromise such as periodic communication, repeated sessions, and application-layer activity. This project provides insight into how SOC and IR teams support detection and investigation. 

---

## 2. Project Relevance 
Wireshark is relevant to this project because it provides a close look at the network activity involved in the simulated command and control behavior. The packet level view makes it possible to see how the traffic moves between systems and how the communication pattern forms during the scenario.

The tool helps bring out details that point toward beaconing or other repeated activity. It shows the timing of the connections, the way the sessions form, and the protocol behavior that appears when a host reaches out to an external system on a steady schedule. These patterns help separate normal traffic from behavior that suggests a controlled or automated exchange.

Its relevance also comes from how the captured packets can be reviewed after the activity takes place. Breaking down the traffic helps outline the flow of the communication and supports the process of identifying indicators that match what is expected in a C2 style exchange. This makes it easier to connect the network behavior to the goals of the project and understand how the simulated activity fits into detection and analysis work

## 3. Methodology
-Yoelbis/Cara

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





