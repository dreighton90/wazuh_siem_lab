# 🛡️ Local SIEM Lab — Wazuh, Docker & Ubuntu 24.04

**Deployed a full open-source SIEM stack from scratch using Wazuh on Docker, configured agent-based endpoint monitoring, and validated threat detection capabilities against MITRE ATT&CK — without bootcamp guidance.**

---

## 🧩 Problem

Most entry-level security candidates claim SIEM experience but have only used pre-configured lab environments. The goal of this project was to deploy a production-grade SIEM stack from scratch — handling every layer of the setup independently, from OS configuration to agent enrollment to dashboard validation — to build genuine operational familiarity with how SIEMs are deployed and maintained in enterprise environments.

---

## 🛠️ Approach

### Architecture
```
Ubuntu 24.04 VM (VirtualBox) → Docker → Wazuh Manager + Dashboard → Wazuh Agent (endpoint)
```

| Component | Role |
|---|---|
| Ubuntu 24.04 | Host OS deployed in VirtualBox |
| Docker + Docker Compose | Container runtime for Wazuh stack |
| Wazuh Manager | Central log collection and correlation engine |
| Wazuh Agent | Endpoint telemetry and event forwarding |
| Wazuh Dashboard | Threat visualization and MITRE ATT&CK mapping |

### Key Technical Decisions

- **Docker over OVA deployment:** Wazuh provides a pre-built OVA for quick setup. Chose Docker instead to build familiarity with container orchestration and to mirror how Wazuh is deployed in cloud-native environments where OVA images aren't an option.
- **Custom ossec.conf edits:** Rather than accepting default agent configuration, manually edited `/var/ossec/etc/ossec.conf` to tune log collection paths and alert thresholds — the same workflow an analyst would follow when onboarding a new endpoint in production.
- **Local-first with AWS expansion path:** Deployed locally to control the full environment and isolate variables during troubleshooting. Architecture is designed to migrate to AWS with minimal changes — Wazuh manager moves to EC2, logs archive to S3, alerts forward to CloudWatch.

---

## ✅ Outcome

| Milestone | Result |
|---|---|
| Docker installed and validated on Ubuntu 24.04 | ✅ Confirmed |
| Wazuh stack deployed via Docker Compose | ✅ All containers running |
| Wazuh agent enrolled and reporting to manager | ✅ Agent active and forwarding |
| Dashboard accessible and displaying telemetry | ✅ Threat intel visible |
| ossec.conf customized for targeted log collection | ✅ Configuration applied |
| MITRE ATT&CK mapping visible in dashboard | ✅ TTPs surfaced |

Full SIEM stack operational end-to-end. Agent telemetry forwarding to manager, alerts visible in dashboard, MITRE ATT&CK technique mapping confirmed active.

---

## 📸 Walkthrough

### Ubuntu & Docker Setup
![Update & Upgrade](project-walkthrough/001_ubuntu_vm_update_upgrade.png)
*System updated and upgraded before installation*

![Docker Install Success](project-walkthrough/01_docker_install_succes.png)
*Docker installed successfully via CLI*

![Docker Running](project-walkthrough/02_docker_installed_running.png)
*Docker service confirmed active and running*

![Docker Compose Installed](project-walkthrough/03_docker_compose_installed.png)
*Docker Compose installed for multi-container orchestration*

![NGINX Running](project-walkthrough/03-container-running-nginx.png)
*Test container running — validated Docker environment before Wazuh deployment*

![NGINX Localhost](project-walkthrough/04-nginx-docker-success-localhost.png)
*Localhost confirmed accessible — networking validated*

### Wazuh Agent + Server
![Agent Install](project-walkthrough/Wazuh_Agent_Install_Redacted.PNG)
*Wazuh agent installed on endpoint*

![Agent Enabled](project-walkthrough/Wazuh_Agent_Enable_and_Stated.png)
*Agent service enabled and started via systemctl*

![ossec.conf](project-walkthrough/Wazuh_Ossec_Config_Redacted.PNG)
*Custom ossec.conf configuration applied for targeted log collection*

![Wazuh CLI Login](project-walkthrough/Wazuh_CLI_Login.png)
*CLI login to Wazuh manager confirmed*

![Server Login](project-walkthrough/Successful_Wazuh_Server_Login_Redacted.PNG)
*Successful authentication to Wazuh server*

![Server Boot Trace](project-walkthrough/Wazuh_Server_booting-Kernel-Trace.png)
*Kernel trace during server boot — verified clean startup*

### Wazuh Dashboard
![Login Screen](project-walkthrough/Wazuh_Login_Screen.png)
*Wazuh dashboard login screen*

![Dashboard Login](project-walkthrough/Wazuh_Dashboard_Login_Redacted.PNG)
*Authenticated to dashboard successfully*

![Dashboard Overview](project-walkthrough/Wazuh_Dashboard_Overview_Redacted.PNG)
*Dashboard displaying agent telemetry and threat intelligence*

---

## 📚 Lessons Learned

**Container networking requires explicit validation before SIEM deployment.** Used an NGINX test container to confirm Docker networking and localhost accessibility before deploying the full Wazuh stack — catching potential networking issues early rather than mid-deployment when the root cause is harder to isolate.

**systemctl errors are diagnostic signals, not blockers.** Hit service start errors during agent configuration that initially looked like installation failures. Working through them — checking logs in `/var/ossec/logs/`, verifying the manager IP in ossec.conf, confirming port accessibility — built the kind of troubleshooting muscle memory that matters in production environments where documentation doesn't cover every edge case.

**Default configurations are a starting point, not a finish line.** The default ossec.conf works, but editing it to target specific log paths made the difference between generic system noise and actionable security telemetry. In a real deployment, every agent config would be tuned to the endpoint's role.

---

## 🔧 Tools & Technologies

- **OS:** Ubuntu 24.04 via VirtualBox
- **SIEM:** Wazuh v4.12
- **Containers:** Docker & Docker Compose
- **Monitoring:** Wazuh Dashboard
- **Framework:** MITRE ATT&CK

---

## 🚀 Planned Extensions

- **AWS migration:** Host Wazuh manager on EC2, archive logs to S3
- **CloudWatch integration:** Forward Wazuh alerts to CloudWatch for cross-platform visibility
- **Compliance rulesets:** Enable PCI-DSS, NIST, and HIPAA rule packs within Wazuh
- **Log shipper:** Add Filebeat for structured log forwarding to OpenSearch
- **Alert pipeline:** Connect to the Cloud Security Alerting System (PostgreSQL) for unified event correlation

---

## ⚠️ Legal Disclaimer

This project was conducted entirely within a locally hosted virtual lab. No public networks, external assets, or production systems were accessed. All content is for educational and professional development purposes only.
