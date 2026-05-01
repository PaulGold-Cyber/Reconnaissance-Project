# 🔎 Reconnaissance Project - scanme.nmap.org

A hands-on cybersecurity project focused on reconnaissance, service analysis, and vulnerability assessment using real-world tools such as Nmap, Shodan, and OSINT techniques.
## Objective
The objective of this project is to perform reconnaissance on a publicly available and authorized target (scanme.nmap.org) in order to identify exposed services, analyze potential risks, and simulate an attacker’s perspective using multiple information gathering techniques.
## Methodology
 The reconnaissance process was conducted in multiple stages
* Active scanning using Nmap to identify open ports and running services
* Passive intelligence gathering using Shodan
* Open-source intelligence (OSINT) using Google Dorks
The collected data was then analyzed to assess potential attack vectors and security risks.
## Tools Used 
* Nmap
* Shodan
* Google Dorks
## Findings
###  Nmap Scan Results
An Nmap scan was performed to identify open ports and services on the target.
```bash
nmap -sV -sC scanme.nmap.org
```
![Nmap Scan](images/nmap_scan.png)
### Key Findings
* Port 22 – SSH (OpenSSH 6.6.1p1)
* Port 80 – HTTP (Apache 2.4.7)
* Port 9929 – Nping Echo
* Port 31337 – tcpwrapped
### Analysis of Exposed Services
**HTTP (Port 80)**
The HTTP service represents the most accessible attack surface. Since it operates over an unencrypted protocol, transmitted data may be exposed to interception attacks such as Man-in-the-Middle (MITM). Additionally, web services are commonly targeted due to the wide range of potential vulnerabilities, including misconfigurations and application layer exploits.

**SSH (Port 22)**
The SSH service allows secure remote administration and is not inherently a vulnerability. However, its exposure to the internet increases the attack surface. Attackers may attempt brute force or credential-based attacks. The detected version appears outdated, which may indicate potential exposure to known vulnerabilities if not properly patched.

**Port 31337 (tcpwrapped)**
The service appears to be filtered or access-controlled. This may indicate the presence of a hidden or restricted service, which could warrant further investigation from an attacker’s perspective.

**Port 9929 (Nping Echo)**
This service is typically used for network diagnostics. While not inherently malicious, exposing such services may provide attackers with additional insight into network behavior and responsiveness.

### Attacker Perspective
From an attacker’s perspective, the initial focus would likely be on the HTTP service (port 80), as web applications provide a broader and more accessible attack surface.
A realistic attack path may involve identifying web-based vulnerabilities or misconfigurations to gain an initial foothold, followed by lateral movement or persistence via services such as SSH.
The exposed SSH service would be considered a high-value target due to its potential for direct system access, while filtered services (e.g., port 31337) may indicate hidden entry points worth further investigation.
## Vulnerability Assessment - Nmap Findings
### Nmap Findings – Slowloris (DoS)
A vulnerability scan using Nmap NSE scripts identified a potential Slowloris Denial-of-Service (DoS) vulnerability affecting the HTTP service.
Slowloris works by maintaining multiple partial HTTP connections to the server, exhausting available resources and preventing legitimate users from accessing the service.
The scan indicates that the target is likely vulnerable to this attack (CVE-2007-6750).
Although this vulnerability does not allow direct system compromise, it can significantly impact availability, which is a critical aspect of security.
The following output from the Nmap vulnerability scan highlights a potential Slowloris Denial-of-Service (DoS) vulnerability affecting the HTTP service:

![Slowloris Vulnerability](images/vuln_scan.png)

This result suggests that the server may be vulnerable to resource exhaustion attacks, which could impact service availability.
## Shodan Insights
Shodan was used to gather additional intelligence about the target.
The results confirmed exposed services and provided insight into the hosting environment and publicly visible metadata. 
Based on the findings, two representative vulnerabilities were selected for analysis due to their relevance to the identified services: 
### Vulnerability 1 - HTTP Request Smuggling
One of the identified potential vulnerabilities is related to HTTP  Request Smuggling. This vulnerability occurs when different servers interpret HTTP requests inconsistently, allowing an attacker to inject malicious requests into the communication chain. Successful exploitation may lead to bypassing security controls, session hijacking, or cache poisoning. This vulnerability is typically dependent on specific server configurations (such as reverse proxies) and may not be exploitable in all environments.
The following Shodan result highlights a potential HTTP Request Smuggling vulnerability affecting Apache servers: 

![HTTP Request Smuggling](images/shodan1.png)

This vulnerability may allow attackers to manipulate HTTP requests and bypass security mechanisms under specific configurations.
### Vulnerability 2 - Buffer Overflow / Memory Corruption
Another identified issue involves a potential buffer overflow vulnerability in Apache modules. Such vulnerabilities may allow an attacker to write data beyond the allocated memory boundaries, potentially leading to application crashes or even remote code execution. However, successful exploitation usually depends on specific modules and configurations being enabled on the server.
The following Shodan result indicates a potential buffer overflow vulnerability in Apache modules:

![Buffer Overflow Vulnerability](images/shodan2.png)

Such vulnerabilities may lead to memory corruption, potentially resulting in application crashes or remote code execution depending on the environment.
Although the identified vulnerabilities affect newer Apache versions, the presence of an outdated version (2.4.7) may still indicate potential exposure to other unpatched vulnerabilities. 
It is important to note that vulnerability data obtained from sources such as Shodan is indicative and requires further validation.
Not all listed vulnerabilities are necessarily exploitable in the specific target environment. 
## Google Dorks insights
Google Dorking techniques did not reveal any sensitive files or exposed directories.
This suggests that the target does not have significant publicly indexed misconfigurations. 
## Conclusion
This reconnaissance project demonstrated how multiple information gathering techniques can be combined to assess the exposure of a target system.
The analysis revealed several exposed services, including SSH and HTTP, each contributing to the overall attack surface.
While no direct remote compromise path was confirmed, the combination of exposed services, outdated software, and potential vulnerabilities suggests realistic attack scenarios. 
In particular, a likely attack path could involve initial access through web-based vectors, followed by attempts to leverage SSH access for persistence. 
This highlights the importance of continuous monitoring, proper configuration, and timely patch management in maintaining a secure environment. 
## Recommendations
Based on the findings, the following security improvements are recommended:
* Implement connection limits and timeouts
* Use a Web Application Firewall (WAF)
* Configure the server to detect and mitigate slow HTTP attacks
* Consider using reverse proxies such as Nginx to handle connections efficiently
* Restrict SSH access to trusted IP addresses
* Enforce strong authentication methods (e.g., key based authentication)
* Keep all services updated and properly patched
## Final Note
This project reflects a structured and analytical approach to reconnaissance, emphasizing not only tool usage but also critical thinking and security oriented analysis.

Screenshots are included to support the findings and provide evidence of the identified services and vulnerabilities.
