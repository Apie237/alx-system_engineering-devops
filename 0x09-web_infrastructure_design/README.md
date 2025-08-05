# 0x09. Web Infrastructure Design

## Task 0: Simple Web Stack

### 📌 Diagram Link
[View Diagram](https://imgur.com/a/your_screenshot_link)  
> *(Replace with your actual image link after uploading your draw.io diagram)*

---

## 🧠 Infrastructure Overview

This task outlines a simple one-server web stack for the domain `www.foobar.com`, using the following components:

- **1 server** (IP: 8.8.8.8)
- **Nginx** as the web server
- **Application server** (e.g., Node.js, PHP-FPM, Gunicorn)
- **Application codebase**
- **MySQL database**
- **Domain name:** `foobar.com` configured with a `www` subdomain pointing to `8.8.8.8` via an A record

---

## 🧩 How It Works (From User to Server)

1. A user opens a browser and types `www.foobar.com`.
2. DNS resolves `www.foobar.com` to the IP address `8.8.8.8` using an **A record**.
3. The user’s request reaches the server (IP: 8.8.8.8).
4. **Nginx (web server)** receives the HTTP request.
5. Nginx forwards the request to the **application server**, which runs the backend code.
6. The application may query the **MySQL database** for data.
7. The response flows back through the app server → Nginx → to the user.

---

## 📖 Key Explanations

### 🔹 What is a Server?
A **server** is a computer or virtual machine that provides services, data, or resources to other computers (clients) over a network.

### 🔹 What is the Role of the Domain Name?
A **domain name** maps a human-readable address (like `foobar.com`) to the numeric IP address (`8.8.8.8`) of your server, allowing users to access it easily.

### 🔹 What Type of DNS Record is `www` in `www.foobar.com`?
It’s an **A record** (Address record), which maps a domain name or subdomain to an IPv4 address.

### 🔹 What is the Role of the Web Server?
The **web server (Nginx)** handles HTTP requests from the client and:
- Serves static files (HTML, CSS, images)
- Forwards dynamic requests to the application server (reverse proxy)

### 🔹 What is the Role of the Application Server?
The **application server** runs backend logic and communicates with the database to generate dynamic content (e.g., PHP, Node.js, Python backends).

### 🔹 What is the Role of the Database?
The **MySQL database** stores structured data (like users, orders, posts) and provides fast access to it when queried by the application server.

### 🔹 What Protocol is Used for Communication?
Communication between the user’s browser and the server happens over the **HTTP** or **HTTPS** protocol, which runs on top of **TCP/IP**.

---

## ⚠️ Limitations of This Infrastructure

| Limitation        | Description |
|-------------------|-------------|
| **SPOF** *(Single Point of Failure)* | If the one server crashes, the entire website goes down. |
| **Downtime During Maintenance** | Any code deployment or server restart will make the website temporarily unavailable. |
| **No Scalability** | The server has limited CPU, RAM, and bandwidth. It cannot handle high traffic or scale horizontally.

---

## 📁 File Structure

