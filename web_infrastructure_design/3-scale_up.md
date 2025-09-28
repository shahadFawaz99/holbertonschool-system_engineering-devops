# 3. Scale Up

## 📌 Infrastructure Description
This infrastructure scales up the previous setup by **adding redundancy** at the load balancer level and **splitting components** into dedicated servers.

- **Domain name:** `foobar.com` with a **www A record** pointing to the load balancers’ public IP.  
- **2 Load balancers (HAProxy) in cluster mode:** Provides redundancy and removes SPOF at the LB level.  
- **Dedicated servers:**
  - **Web server(s):** Nginx only.  
  - **Application server(s):** Runs the business logic/code.  
  - **Database server(s):** Primary-Replica MySQL cluster for persistence and redundancy.  
- **+1 extra server:** Added to increase capacity (could be for app tier or web tier).  

---

## 📌 Why Adding New Elements
- **Extra server:** Supports higher traffic and improves redundancy.  
- **Second load balancer:** Removes the load balancer SPOF, ensures failover.  
- **Splitting components:**  
  - **Web server:** Optimized for handling static files, incoming HTTP(S) traffic.  
  - **Application server:** Focused on dynamic content and logic execution.  
  - **Database server:** Dedicated to persistence and replication, optimized for queries.  

---

## 📌 Application Server vs Web Server
- **Web server (Nginx):**  
  - Handles client HTTP/HTTPS requests.  
  - Serves static files (HTML, CSS, JS, images).  
  - Passes dynamic requests to the application server.  

- **Application server:**  
  - Executes application logic (Python, PHP, Ruby, etc.).  
  - Communicates with the database.  
  - Returns processed results back to the web server.  

---

## 📌 Diagram

```mermaid
flowchart TD
    A[User] --> B[DNS: www.foobar.com -> LB Cluster IP]
    B --> LB1[Load Balancer 1 - HAProxy]
    B --> LB2[Load Balancer 2 - HAProxy]

    LB1 <---> LB2

    LB1 --> W1[Web Server 1 - Nginx]
    LB2 --> W2[Web Server 2 - Nginx]

    W1 --> APP1[Application Server 1]
    W2 --> APP2[Application Server 2]
    APP1 --> DBP[MySQL - Primary DB]
    APP2 --> DBR[MySQL - Replica DB]

    DBP <-.replicates to.-> DBR
