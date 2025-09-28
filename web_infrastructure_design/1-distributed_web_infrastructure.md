# 1. Distributed Web Infrastructure

## 📌 Infrastructure Description
This infrastructure improves availability and scalability by introducing a **load balancer** and multiple servers.

- **Domain name:** `foobar.com` with a **www A record** pointing to the load balancer’s public IP.  
- **Load balancer (HAProxy):** Distributes traffic across two servers.  
- **2 servers (each containing):**
  - Web server (Nginx)
  - Application server
  - Application files (code base)
  - Database (MySQL)

---

## 📌 Why Adding New Elements
- **Load balancer:** Ensures requests are distributed across servers, preventing overload on one machine.  
- **Multiple servers:** Provide redundancy and allow the system to handle more traffic.  
- **Database replication:** Avoids single database failure, supports read scaling.  

---

## 📌 Key Concepts
- **Load balancer algorithms:**  
  Example: **Round Robin** (each request goes to the next server in order).  
  Other options: Least Connections, IP Hash.  

- **Active-Active vs Active-Passive:**  
  - **Active-Active:** Both servers handle traffic simultaneously.  
  - **Active-Passive:** One server handles traffic, the other stays on standby (used for failover).  

- **Database Primary-Replica (Master-Slave):**  
  - **Primary node:** Accepts both **reads and writes**.  
  - **Replica node:** Accepts **reads only**, replicates data from the primary.  
  - This improves performance and ensures redundancy.  

---

## 📌 Issues with This Infrastructure
- **SPOF:**  
  - Load balancer itself is a single point of failure.  
  - Database Primary node is still a SPOF (writes depend on it).  

- **Security issues:**  
  - No firewalls configured.  
  - No HTTPS, traffic is unencrypted.  

- **No monitoring:**  
  - No visibility into server health, performance, or traffic.  

---

## 📌 Diagram

```mermaid
flowchart TD
    A[User] --> B[DNS: www.foobar.com -> LB IP]
    B --> C[Load Balancer - HAProxy]

    C --> S1[Server 1]
    C --> S2[Server 2]

    subgraph S1 [Server 1]
        W1[Nginx - Web Server]
        A1[Application Server]
        F1[Application Files]
        DB1[MySQL - Primary DB]
    end

    subgraph S2 [Server 2]
        W2[Nginx - Web Server]
        A2[Application Server]
        F2[Application Files]
        DB2[MySQL - Replica DB]
    end

    W1 --> A1 --> F1
    A1 --> DB1
    W2 --> A2 --> F2
    A2 --> DB2

    DB1 <-.replicates to.-> DB2
