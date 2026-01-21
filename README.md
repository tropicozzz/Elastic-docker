# 🛡️ Secure Elastic Stack (ELK) Deployment with Hybrid Fleet Server

![Elastic Stack](https://img.shields.io/badge/Elastic_Stack-8.x-005571?style=for-the-badge&logo=elasticsearch)
![Docker](https://img.shields.io/badge/Docker-Enabled-2496ED?style=for-the-badge&logo=docker)
![SSL/TLS](https://img.shields.io/badge/Security-SSL%2FTLS-green?style=for-the-badge&logo=letsencrypt)
![Linux](https://img.shields.io/badge/Platform-Linux-FCC624?style=for-the-badge&logo=linux)

This project implements a robust security and monitoring architecture using the **Elastic Stack**. The primary goal was to simulate a production-grade environment where security is paramount, implementing full SSL/TLS encryption across all components and utilizing a hybrid architecture for agent management.

## 📋 Project Architecture

The deployment was designed with a hybrid architecture to maximize flexibility and simulate real-world enterprise scenarios where not every component can be containerized.

1.  **Data & Visualization Layer (Dockerized):**
    * **Elasticsearch:** Search and analytics engine running in a container with data persistence and security enabled (xpack).
    * **Kibana:** Visualization interface, securely connected to Elasticsearch.
2.  **Agent Management Layer (Native/Host):**
    * **Elastic Fleet Server:** Executed directly on the operating system (Linux), outside of Docker. This allows for direct control over host resources and simplifies system-level certificate management for remote agents.
3.  **Security (SSL/TLS):**
    * Encrypted communication between Elasticsearch and Kibana.
    * Encrypted communication between Elastic Agents and the Fleet Server.
    * CA Certificate verification to ensure trust between all components.

---

## 🚀 Key Features

* **Streamlined Updates:** By containerizing the core stack, version upgrades are decoupled from the host OS. Updating Elasticsearch or Kibana is as simple as changing the Docker image tag, eliminating dependency conflicts and downtime.
* **End-to-End Security:** Generation of CA and instance certificates to enforce HTTPS traffic across the entire pipeline.
* **Docker Compose Orchestration:** Rapid and reproducible deployment of the data and visualization layers.
* **Centralized Management:** Use of Fleet to manage agent policies remotely.
* **Hybrid Integration:** Network configuration allowing the Fleet Server (Host) to communicate with Elasticsearch (Container) via the Docker network or secure `localhost`.

---

## 🛠️ Tech Stack

* **Docker & Docker Compose:** Containerization of core services for isolation and easy maintenance.
* **Elasticsearch 8.x:** Log/Metric storage and analysis.
* **Kibana 8.x:** Dashboarding and Security management.
* **Elastic Agent & Fleet:** Data ingestion and endpoint management.
* **OpenSSL / Elastic Certutil:** PKI (Public Key Infrastructure) generation.

---

## ⚙️ Setup & Deployment Overview

### 1. Certificate Generation
Native Elastic tools were used to create a Certificate Authority (CA) and sign certificates for nodes and clients.

```bash
# Example of cert generation (inside setup container)
bin/elasticsearch-certutil cert --silent --in instances.yml --out certs/bundle.zipg
