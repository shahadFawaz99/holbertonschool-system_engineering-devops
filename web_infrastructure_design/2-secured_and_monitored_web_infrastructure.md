# 2. Secured and Monitored Web Infrastructure

## 📌 Infrastructure Description
This setup builds on the distributed infrastructure, but now it is **secured**, supports **HTTPS**, and is **monitored**.

- **Domain name:** `foobar.com` with a **www A record** pointing to the load balancer’s IP.  
- **Load balancer (HAProxy)** with an **SSL certificate** to serve HTTPS traffic.  
- **2 servers (each containing):**
  - Web server (Nginx)
  - Application server
  - Application files (code base)
  - Database (MySQL Primary-Replica)  
- **3 Firewalls:** One protecting the load balancer, one protecting the servers, and one protecting the database.  
- **3 Monitoring clients:** Installed on each server to collect metrics and logs, sent to a monitoring system (e.g., SumoLogic).  

---

## 📌 Why Adding New Elements
- **Firewalls:** Restrict unauthorized access; allow only necessary ports (80/443 for web, 3306 for DB).  
- **SSL certificate:** Encrypts traffic between the user and the website (protects sensitive data).  
- **Monitoring clients:** Provide visibility into system health, errors, usage, and QPS.  

---

## 📌 Key Concepts
- **Firewalls:** Block malicious traffic, enforce security rules.  
- **HTTPS with SSL/TLS:** Encrypts communication to ensure confidentiality and integrity.  
- **Monitoring:** Detects failures, bottlenecks, and anomalies. Collects logs and metrics.  
- **How monitoring collects data:** Agents/clients installed on servers send logs, metrics (CPU, RAM, QPS) to a central monitoring service.  
- **Monitoring QPS (Queries per Second):** Configure the monitoring tool to track requests served by Nginx.  

---

## 📌 Issues with This Infrastructure
- **SSL termination at the load balancer:**  
  Traffic between load balancer and servers is unencrypted → still vulnerable inside the private network.  

- **Database write bottleneck:**  
  Only one Primary MySQL server accepts writes → performance and availability risk.  

- **Servers with identical components:**  
  Each server has web, app, and DB → not modular; scaling and maintenance are harder.  

---

## 📌 Diagram

```mermaid
flowchart TD
    A[User] --> B[DNS: www.foobar.com -> LB IP]
    B --> LB[Load Balancer - HAProxy + SSL Certificate]

    subgraph Firewall1 [Firewall - LB Protection]
        LB
    end

    LB --> FW2[Firewall - App Servers Protection]
    FW2 --> S1[Server 1]
    FW2 --> S2[Server 2]

    subgraph S1 [Server 1]
        W1[Nginx - Web Server]
        A1[Application Server]
        F1[Application Files]
        DB1[MySQL - Primary DB]
        M1[Monitoring Client]
    end

    subgraph S2 [Server 2]
        W2[Nginx - Web Server]
        A2[Application Server]
        F2[Application Files]
        DB2[MySQL - Replica DB]
        M2[Monitoring Client]
    end

    subgraph FW3 [Firewall - Database Protection]
        DB1
        DB2
    end

    M1 --> MON[Monitoring System]
    M2 --> MON[Monitoring System]
    MON[Central Monitoring]

    DB1 <-.replicates to.-> DB2
