# AIOps Automation Solution with Ansible Automation Platform & LlamaStack on OpenShift

## 📌 Overview
This project demonstrates an **end-to-end AIOps automation framework** built using:
- **Ansible Automation Platform (AAP)**
- **OpenShift AI**
- **LLamaStack Agent**
- **Ansible Lightspeed**

The solution automates detection, analysis, notification, and remediation of node/service failures (e.g., HTTP service downtime).

---

## ⚙️ Workflow
<img src="./images/Final-Architecture.png" alt="Architecture" height="400" />

This workflow demonstrates how Event-Driven Ansible (EDA), AI Insights, and Lightspeed integrate with Ansible Automation Platform (AAP) to detect, analyze, and remediate service outages using a combination of autonomous agents and human-in-the-loop automation.

### ✅ Prerequisites

Before you begin, ensure you have the following configured:

- **LlamaStack** on OpenShift → [OpenShift setup steps](./openshift/README.md)  
- **Ansible Automation Platform (AAP)** → [AAP setup steps](./AAP/Readme.md)  