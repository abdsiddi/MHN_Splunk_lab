# Modern Honeypot Network (MHN) & Splunk Integration Lab

![Build Status](https://img.shields.io/badge/Status-Deprecated%2FProof_of_Concept-red)
![Platform](https://img.shields.io/badge/Platform-AWS_EC2-FF9900)
![OS](https://img.shields.io/badge/OS-Ubuntu_18.04-E95420)

## 🛑 Project Disclaimer
**Status:** Deprecated  
*I highly recommend not using MHN for production or modern deployments. It is no longer officially supported, relies on outdated dependencies (like Python 2.7), and requires significant manual troubleshooting to function.* [cite: 983] [cite_start]*This repository serves purely as a proof-of-concept homelab to demonstrate honeypot deployment, legacy system troubleshooting, and SIEM integration.* [cite: 982]

## 📖 Overview
[cite_start]This project demonstrates the deployment of a Modern Honeypot Network (MHN) server on AWS, the deployment of a Dionaea honeypot sensor, and the custom integration of attack logs into Splunk Enterprise for threat analysis and geo-location mapping. [cite: 9, 522, 596, 895]

### Team Members
* [cite_start]M. Abdullah Siddiqui [cite: 4]
* [cite_start]Salman Saeed [cite: 5]
* [cite_start]Umar Khan [cite: 6]
* [cite_start]Zia Ullah [cite: 7]
* [cite_start]Mehboob [cite: 8]

---

## 🏗️ Architecture & Infrastructure
* [cite_start]**Cloud Provider:** Amazon Web Services (AWS) [cite: 9]
* [cite_start]**Instance Type:** `t3.micro` (1GB RAM, 15GB Disk Space) [cite: 21, 54]
* [cite_start]**Operating System:** Ubuntu 18.04 LTS (Required due to MHN compatibility limitations) [cite: 10, 18]
* [cite_start]**Honeypot Sensor:** Dionaea [cite: 522]
* [cite_start]**SIEM:** Splunk Enterprise (Local VM) [cite: 596]

### Security Group (Inbound Rules)
[cite_start]To ensure proper communication, the following ports were opened[cite: 99]:
* [cite_start]**HTTPS:** 443 (TCP) [cite: 141, 142, 143]
* [cite_start]**SSH:** 22 (TCP) [cite: 147, 148, 149]
* [cite_start]**HTTP:** 80 (TCP) [cite: 159, 160, 161]
* [cite_start]**Custom TCP:** 8181 [cite: 165, 166, 167]
* [cite_start]**All Traffic:** Allowed for honeypot sensor traffic [cite: 153, 154, 155]

![AWS EC2 Security Groups](./images/aws-security-groups.png) 
[cite_start]*(Note: Replace with your inbound rules screenshot)* [cite: 102]

---

## 🛠️ Deployment Steps & Troubleshooting

### 1. MHN Server Installation
[cite_start]The MHN installation requires a Python 2.7 virtual environment. [cite: 190]
[cite_start]During the setup script execution, several legacy services failed to start. [cite: 255, 256, 259]

**Troubleshooting Supervisor Errors:**
[cite_start]The `mhn-collector` and `mnemosyne` services exited fatally. [cite: 266, 267]
* **Fix:** The configuration files were missing from the deployment. [cite_start]I had to manually create `/opt/mhn/mhn-collector.py` using archived source code and update the supervisor configuration (`/etc/supervisor/conf.d/mhn-collector.conf`). [cite: 268, 273, 300]
* [cite_start]After reloading supervisorctl, all services transitioned to `RUNNING`. [cite: 301, 305, 315]

### 2. Fixing the MHN Web UI
[cite_start]Upon initial access, the MHN dashboard failed to render properly, preventing login. [cite: 367]
* **Fix:** CSS and JS dependencies were broken. [cite_start]I accessed the archived Pwnlandia GitHub repository to manually download and restore the missing static files into `/server/mhn/static/`. [cite: 367, 372, 378, 406]

![MHN Dashboard](./images/mhn-dashboard-working.png)
[cite_start]*(Note: Replace with your working MHN login/dashboard screenshot)* [cite: 422, 429]

### 3. Deploying the Dionaea Honeypot
[cite_start]Dionaea was selected as the honeypot sensor. [cite: 522]
[cite_start]Deployment was executed via a `wget` bash script generated directly from the MHN UI and executed on the target environment. [cite: 533]

![Dionaea Deployment](./images/dionaea-deploy.png)
[cite_start]*(Note: Replace with your honeypot deployment terminal screenshot)* [cite: 530]

---

## 📊 Splunk SIEM Integration

[cite_start]While MHN captures logs internally (`/var/log/mhn/mhn-splunk.log`), a custom integration was required to push this data to a local Splunk Enterprise instance. [cite: 596, 740, 746]

### Splunk Configuration
1. [cite_start]Installed Splunk 9.4.2 via `.deb` package on a local Ubuntu VM. [cite: 598]
2. [cite_start]Configured a **HTTP Event Collector (HEC)** in Splunk. [cite: 687]
3. [cite_start]Generated an access token and enabled HEC globally on port `8088`. [cite: 685, 725, 732]

![Splunk HEC Setup](./images/splunk-hec.png)
[cite_start]*(Note: Replace with your Splunk Edit Token screenshot)* [cite: 692]

### Custom Python Forwarder
[cite_start]I authored a custom Python script (`splunk_forwarder.py`) to continuously read the MHN log file and push events to the Splunk HEC endpoint via HTTP POST requests. [cite: 746, 757, 762]

```python
import time
import requests

splunk_url = 'http://<YOUR-SPLUNK-IP>:8088/services/collector/event'
splunk_token = '<YOUR-HEC-TOKEN>'
log_file = '/var/log/mhn/mhn-splunk.log'

headers = {
    'Authorization': f'Splunk {splunk_token}',
    'Content-Type': 'application/json'
}

with open(log_file, 'r') as f:
    f.seek(0, 2) # Go to end of file
    while True:
        line = f.readline()
        if not line:
            time.sleep(0.5)
            continue
        # Request handling logic here