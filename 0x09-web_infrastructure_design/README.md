# 0x09. Web Infrastructure Design

This repository contains whiteboard designs and explanations for two foundational web infrastructure setups: a simple single-server stack and a distributed three-server system. The goal is to understand how web applications are deployed, scaled, and made resilient in real-world scenarios.

---

## 📁 Project Structure

- `0-simple_web_stack`: One-server web infrastructure design
- `1-distributed_web_infrastructure`: Distributed multi-server infrastructure with load balancing and replication

---

## 📌 0. Simple Web Stack

### 🧱 Components Used

- 1 **Server** (IP: `8.8.8.8`)
- 1 **Web Server**: Nginx
- 1 **Application Server**
- 1 **Database**: MySQL
- 1 **Codebase**
- 1 **Domain Name**: `www.foobar.com`

### 🌐 Flow Explanation

1. The **user** sends a request to `www.foobar.com`.
2. A **DNS lookup** translates `www.foobar.com` to IP `8.8.8.8` (a `CNAME` or `A` record).
3. The request reaches the **web server (Nginx)** on the server.
4. Nginx forwards the request to the **application server**.
5. The **application** (e.g., PHP, Python) queries the **MySQL database** if needed.
6. The app returns data to the **web server**, which then responds to the **user**.

### 📖 Key Concepts

- **Server**: A physical or virtual machine running services (Nginx, MySQL, etc.)
- **Domain Name**: Human-readable alias for an IP address.
- **DNS Record**: The `www` in `www.foobar.com` is typically a **CNAME** or **A** record.
- **Web Server**: Handles HTTP requests and responses (Nginx).
- **Application Server**: Runs dynamic content and business logic.
- **Database**: Stores persistent application data.
- **Communication**: HTTP(S) over TCP/IP from browser to server.

### ⚠️ Issues

- **SPOF (Single Point of Failure)**: Only one server — if it crashes, everything is down.
- **Maintenance Downtime**: Restarting any part of the stack causes unavailability.
- **No Scalability**: Cannot handle traffic spikes.

📷 Whiteboard Diagram Screenshot:  
[Simple Web Stack Diagram](https:https://imgur.com/a/k0wR6Dx)

---

## 📌 1. Distributed Web Infrastructure

### 🧱 Components Used

- 3 **Servers**:
  - 2 **App/Web Servers**
  - 1 **Load Balancer** (HAProxy)
- 1 **Web Server** (Nginx)
- 1 **Application Server**
- 1 **Load Balancer** (HAProxy)
- 1 **MySQL Database** (Primary/Replica setup)
- 1 **Codebase**

### 🌐 Flow Explanation

1. The **user** requests `www.foobar.com`.
2. DNS resolves the domain to the **Load Balancer IP**.
3. The **load balancer (HAProxy)** distributes the traffic to one of the two backend servers.
4. The **web server** (Nginx) handles the request and passes it to the **application server**.
5. The **application** queries the **Primary MySQL DB** (writes) or **Replica** (reads).
6. The response flows back to the user.

### ⚙️ Load Balancer

- **Algorithm**: Round Robin (default)
- **How it works**: Alternates requests between servers to distribute load evenly.

### 🔄 Active-Active vs Active-Passive

- **Active-Active**: All servers are live and handle traffic simultaneously.
- **Active-Passive**: Only one server handles traffic; the other stands by for failover.
- This setup is **Active-Active** for web/app servers, but **Primary-Replica** for the DB.

### 🗄️ Primary-Replica Database

- **Primary (Master)**: Handles writes/updates.
- **Replica (Slave)**: Syncs from master, handles reads.
- Improves performance and availability.

### ⚠️ Issues

- **SPOF**: Load balancer (if not replicated).
- **Security**: No firewall or HTTPS in current setup.
- **Monitoring**: No visibility or alerting systems.

📷 Whiteboard Diagram Screenshot:  
[Distributed Web Infrastructure Diagram](https://imgur.com/a/RjgoKaE)

---

## 🔗 Repository Structure

