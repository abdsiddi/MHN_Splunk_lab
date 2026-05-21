<div align="center">

<img src="https://img.shields.io/badge/AWS-Cloud-orange?logo=amazonaws&logoColor=white">
<img src="https://img.shields.io/badge/Honeypot-MHN-blue">
<img src="https://img.shields.io/badge/SIEM-Splunk-000000?logo=splunk&logoColor=white">
<img src="https://img.shields.io/badge/Dionaea-Honeypot-green">
<img src="https://img.shields.io/badge/Ubuntu-18.04-orange?logo=ubuntu&logoColor=white">

<br><br>

# Modern Honey Network (MHN) + Splunk Integration

**Threat Intelligence Honeypot Lab on AWS**

`HONEYPOT DEPLOYMENT · TROUBLESHOOTING · SIEM INTEGRATION · REAL-TIME VISUALIZATION`

</div>

---

### Author
**Muhammad Abdullah Siddiqui**


### Environment
AWS EC2 (Ubuntu 18.04) + Local VMware Workstation

### Stack
Modern Honey Network (MHN) · Dionaea · Splunk Enterprise

---

## Project Overview

This project demonstrates the deployment of **Modern Honey Network (MHN)** on AWS, configuration of the **Dionaea** honeypot, and integration with **Splunk Enterprise** for log collection, analysis, and visualization.

---

## 📂 Contents

1. [AWS Infrastructure Setup](#01-aws-infrastructure-setup)
2. [MHN Server Installation & Troubleshooting](#02-mhn-server-installation--troubleshooting)
3. [Dionaea Honeypot Deployment](#03-dionaea-honeypot-deployment)
4. [Splunk Enterprise Setup & Integration](#04-splunk-enterprise-setup--integration)
5. [Results & Visualizations](#05-results--visualizations)
6. [Recommendations](#06-recommendations)

---

## 01. AWS Infrastructure Setup

- Chose Ubuntu 18.04 LTS because newer versions are not compatible with MHN.
- Instance: 1 GB RAM, 15 GB disk space.
- Created SSH key pair for access from local VM.
- Configured Inbound & Outbound security rules.

![AWS EC2 Instance Launch](images/01-aws-ec2.png)

![SSH Key Download](images/02-key-download.png)

![Security Group Rules](images/03-security-groups.png)

![Successful SSH Connection](images/04-ssh-access.png)

---

## 02. MHN Server Installation & Troubleshooting

- Used Python 2.7 in virtual environment due to outdated MHN.
- Ran `install.sh`.
- Fixed missing supervisor configuration files.
- Resolved `mhn-collector` and `mhn-ui` spawn errors.
- Manually downloaded missing CSS files from GitHub.

![MHN Installation](05-mhn-install.png)

![Supervisor Errors](images/06-supervisor-error.png)

![Services Status After Fix](images/07-services-running.png)

**MHN UI Issues & Fix:**

![MHN UI Broken CSS](images/08-mhn-ui-broken.png)

![Missing CSS Files from GitHub](images/09-github-css.png)

![Final Working MHN Dashboard](images/10-mhn-ui-working.png)

---

## 03. Dionaea Honeypot Deployment

Deployed Dionaea using MHN deployment script. Successfully started capturing attacks.

![Dionaea Deployment Command](images/11-dionaea-deploy.png)

![Live Attacks in MHN](images/12-mhn-attacks.png)

**Real-time World Map:**

![World Map Visualization](images/13-world-map.png)

---

## 04. Splunk Enterprise Setup & Integration

- Installed Splunk Enterprise on local VM.
- Configured HTTP Event Collector (HEC) with token.
- Created `forward.py` script to send MHN logs to Splunk.

![Splunk Installation](images/14-splunk-install.png)

![Splunk Login](images/15-splunk-login.png)

![HEC Token Configuration](images/16-hec-config.png)

![Forward.py Script](images/17-forward-py.png)

![Successful Log Forwarding](images/18-forwarding-success.png)

---

## 05. Results & Visualizations

**Splunk Search Results:**

![Splunk Logs Search](images/19-splunk-logs.png)

![Geo Location Search](images/20-splunk-geo.png)

![Final Splunk Dashboard](images/21-splunk-final.png)

---

## 06. Recommendations

> **⚠️ Important Note**

**I would recommend not to use MHN** since it is no longer supported. It is too much of a hassle due to outdated dependencies, missing files, and frequent issues. While it served as a good learning experience, please use modern alternatives for future projects.

---

## Technologies Used

- **Cloud**: AWS EC2
- **OS**: Ubuntu 18.04 LTS
- **Honeypot**: Modern Honey Network (MHN) + Dionaea
- **SIEM**: Splunk Enterprise 9.4.2
- **Scripting**: Python 2.7, Bash

---

<div align="center">

**Cyber Threat Intelligence Home Lab**  
*Successfully Deployed & Integrated*

</div>
