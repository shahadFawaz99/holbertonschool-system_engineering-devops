# 0. Simple Web Stack

## 📌 Infrastructure Description
This simple infrastructure hosts the website **www.foobar.com** on a single server.

- **Domain name:** `foobar.com` configured with a **www A record** pointing to `8.8.8.8`.
- **Server components:**
  - **Web server (Nginx)** → handles HTTP requests, serves static content, forwards requests.
  - **Application server** → executes application code (business logic).
  - **Application files (code base)** → the website’s code.
  - **Database (MySQL)** → stores persistent data.

---

## 📌 Components Explanation
- **What is a server?**  
  A physical or virtual machine that provides services (e.g., hosting a website) to clients.

- **Role of the domain name:**  
  Translates a human-readable address (`foobar.com`) into the server IP (`8.8.8.8`).

- **Type of DNS record (`www`):**  
  An **A record**, mapping `www.foobar.com` to the IP address of the server.

- **Role of the web server:**  
  Handles HTTP requests, serves static files, and forwards dynamic requests to the application server.

- **Role of the application server:**  
  Runs the application code and generates dynamic content.

- **Role of the database:**  
  Stores and retrieves structured data (users, posts, transactions, etc.).

- **Server communication with user’s computer:**  
  Uses **HTTP/HTTPS protocols** over TCP/IP.

---

## 📌 Issues with This Infrastructure
- **SPOF (Single Point of Failure):** If the server crashes, the entire site is down.  
- **Downtime during maintenance:** Any updates or restarts make the website unavailable.  
- **Scalability limits:** One server cannot handle heavy traffic loads.

---

## 📌 Diagram

```mermaid
flowchart TD
    A[User] --> B[DNS: www.foobar.com -> 8.8.8.8]
    B --> C[Server 8.8.8.8]

    subgraph C [Single Server]
        D[Nginx - Web Server]
        E[Application Server]
        F[Application Files]
        G[MySQL Database]
    end

    D --> E
    E --> F
    E --> G
