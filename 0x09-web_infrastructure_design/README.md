# 0x09. Web Infrastructure Design

This repository contains whiteboard designs and explanations for four foundational web infrastructure setups: a simple single-server stack, a distributed three-server system, a secured and monitored infrastructure, and a scaled-up enterprise architecture. The goal is to understand how web applications are deployed, scaled, secured, and made resilient in real-world scenarios.

---

## 📁 Project Structure

- `0-simple_web_stack`: One-server web infrastructure design
- `1-distributed_web_infrastructure`: Distributed multi-server infrastructure with load balancing and replication
- `2-secured_and_monitored_web_infrastructure`: Three-server secured infrastructure with firewalls, SSL, and monitoring
- `3-scale_up`: Enterprise-grade scaled infrastructure with component separation and load balancer clustering

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
[Simple Web Stack Diagram](https://imgur.com/a/k0wR6Dx)

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

## 📌 2. Secured and Monitored Web Infrastructure

### 🧱 Components Used

- 3 **Servers** (each with identical components)
- 3 **Firewalls** (one per server)
- 1 **SSL Certificate** (for HTTPS)
- 1 **Load Balancer** (with SSL termination)
- 3 **Web Servers** (Nginx)
- 3 **Application Servers**
- 3 **Databases** (MySQL)
- 3 **Monitoring Clients** (Sumologic/DataDog agents)

### 🌐 Flow Explanation

1. **User** makes HTTPS request to `www.foobar.com`.
2. **DNS** resolves to load balancer IP.
3. **Firewall** filters malicious traffic before load balancer.
4. **Load Balancer** terminates SSL and distributes requests.
5. **Server Firewalls** provide additional filtering per server.
6. **Web/App Servers** process requests and query databases.
7. **Monitoring Agents** continuously collect metrics and logs.

### 🔒 Security Components

#### Firewalls
- **Purpose**: Filter network traffic and block malicious requests
- **Placement**: One before load balancer, one per server
- **Function**: Access control, attack prevention, network segmentation

#### SSL Certificate
- **Purpose**: Enable HTTPS encryption for data protection
- **Benefits**: Data encryption, server authentication, user trust
- **Location**: Installed on load balancer

### 📊 Monitoring

#### What Monitoring Is Used For
- **Performance Tracking**: Response times, resource usage, throughput
- **Issue Detection**: Early problem identification
- **Security Monitoring**: Detect attacks and unusual activity
- **Capacity Planning**: Understanding scaling needs

#### How Monitoring Collects Data
- **Agents**: Software on each server collects system metrics
- **Log Analysis**: Web server logs, application errors
- **Network Monitoring**: Latency, connectivity, packet loss
- **Custom Metrics**: Business-specific KPIs

#### Monitoring Web Server QPS
1. Configure Nginx access logs with timestamps
2. Use monitoring agent to parse log files
3. Count requests per second from logs
4. Create QPS dashboard in monitoring tool
5. Set up alerting for abnormal QPS levels

### ⚠️ Issues

#### SSL Termination at Load Balancer
- **Problem**: Unencrypted traffic between load balancer and servers
- **Risk**: Internal network vulnerability
- **Better Approach**: End-to-end encryption or SSL pass-through

#### Single MySQL Write Server
- **Problem**: All write operations go through one database server
- **Risk**: Performance bottleneck and single point of failure
- **Better Approach**: Master-master replication or database clustering

#### Identical Server Components
- **Problem**: Resource waste, larger attack surface, maintenance complexity
- **Risk**: Components compete for resources, inefficient scaling
- **Better Approach**: Separate database servers, specialized roles

📷 Whiteboard Diagram Screenshot:  
[Secured and Monitored Infrastructure Diagram](https://imgur.com/a/gDLY1xF)

---

## 📌 3. Scale Up Infrastructure

### 🧱 Components Used

- 4 **Servers**:
  - 2 **Load Balancers** (HAProxy cluster)
  - 1 **Web Server** (Nginx)
  - 1 **Application Server**
  - 1 **Database Server** (MySQL)
- **Component Separation**: Each service type on dedicated servers

### 🌐 Flow Explanation

1. **User** makes HTTPS request to `www.foobar.com`.
2. **DNS** resolves to load balancer cluster Virtual IP (VIP).
3. **Active Load Balancer** (HAProxy) receives and distributes request.
4. **Web Server** (Nginx) handles static content or forwards dynamic requests.
5. **Application Server** processes business logic and dynamic content.
6. **Database Server** provides data for application queries.
7. Response flows back through the chain to the user.

### 🔄 Load Balancer Cluster

#### Why Add Second Load Balancer
- **High Availability**: Eliminates load balancer as single point of failure
- **Redundancy**: Automatic failover if primary load balancer fails
- **Maintenance**: Zero-downtime updates and maintenance
- **Performance**: Can handle more concurrent connections

#### Configuration
- **Active-Passive Setup**: One active, one standby HAProxy
- **Virtual IP (VIP)**: Shared IP that floats between load balancers
- **Health Checks**: Continuous monitoring and automatic failover

### 🏗️ Component Separation

#### Why Separate Components
- **Performance Isolation**: No resource competition between services
- **Independent Scaling**: Scale each tier based on specific demand
- **Specialization**: Optimize each server for its specific workload
- **Maintenance**: Update components without affecting others
- **Security**: Different security zones for different service types

#### Web Server (Nginx) - Dedicated
- **Static Content**: Optimized for serving files, images, CSS, JavaScript
- **SSL Termination**: Handles encryption/decryption efficiently
- **Reverse Proxy**: Forwards dynamic requests to application server
- **Caching**: Advanced caching strategies for performance
- **Load Distribution**: Can balance requests across multiple app servers

#### Application Server - Dedicated
- **Business Logic**: Focused on executing application code
- **Dynamic Processing**: Handles PHP, Python, Node.js, Java applications
- **Database Interaction**: Manages database queries and transactions
- **Session Management**: Handles user sessions and application state
- **API Processing**: Manages REST/GraphQL API endpoints

#### Database Server - Dedicated
- **Data Management**: Specialized for database operations only
- **Storage Optimization**: Uses specific hardware (SSDs, RAID) for performance
- **Memory Management**: Optimized RAM allocation for database caching
- **Backup Operations**: Dedicated resources for backup and recovery
- **Replication**: Primary-replica setup for high availability

### 📊 Web Server vs Application Server

| **Aspect** | **Web Server (Nginx)** | **Application Server** |
|------------|------------------------|------------------------|
| **Primary Function** | Serves static content | Executes dynamic code |
| **Content Type** | HTML, CSS, JS, images | Generated content |
| **Processing** | File serving, routing | Business logic execution |
| **Resource Usage** | I/O intensive | CPU/Memory intensive |
| **Scalability** | Easy horizontal scaling | Requires careful scaling |
| **Examples** | Nginx, Apache | Gunicorn, Tomcat, PM2 |
| **Configuration** | Simple routing rules | Complex application setup |

### 🚀 Architecture Benefits

#### Performance Benefits
- **Specialized Hardware**: Each server optimized for its role
- **Parallel Processing**: Components work simultaneously
- **Resource Efficiency**: No waste of resources on unused services
- **Cache Optimization**: Web server handles caching effectively

#### Scalability Benefits
- **Independent Scaling**: Add servers where needed most
- **Horizontal Growth**: Scale each tier based on demand patterns
- **Cost Optimization**: Use appropriate hardware for each function
- **Future-Proof**: Easy to add more servers of any type

#### Availability Benefits
- **Fault Isolation**: Component failure doesn't cascade
- **Load Balancer Redundancy**: No single point of failure for traffic
- **Maintenance Windows**: Independent component maintenance
- **Disaster Recovery**: Better recovery with separated services

### ⚠️ Considerations

#### Complexity
- **More Components**: More servers to manage and monitor
- **Network Communication**: Increased inter-server communication
- **Configuration**: More complex setup and coordination

#### Cost
- **Additional Servers**: Higher infrastructure costs
- **Management Overhead**: More resources needed for administration
- **Monitoring**: More comprehensive monitoring required

📷 Whiteboard Diagram Screenshot:  
[Scale Up Infrastructure Diagram](https://imgur.com/a/wajIoC0)

---

## 🔗 Repository Structure

```
0x09-web_infrastructure_design/
├── README.md
├── 0-simple_web_stack
├── 1-distributed_web_infrastructure
├── 2-secured_and_monitored_web_infrastructure
└── 3-scale_up
```

---

## 🎯 Learning Objectives

By completing these projects, you should be able to:

- Draw comprehensive web infrastructure diagrams
- Explain the role of each component in a web stack
- Understand system redundancy and high availability concepts
- Identify single points of failure and scalability issues
- Explain security measures like firewalls and HTTPS
- Understand monitoring and its importance in production systems
- Know key acronyms: **LAMP**, **SPOF**, **QPS**

---

## 🚀 Key Takeaways

### Evolution of Web Infrastructure
1. **Simple Stack**: Single server with all components - easy but risky
2. **Distributed**: Multiple servers with load balancing - better performance and availability
3. **Secured & Monitored**: Added security layers and observability - production-ready
4. **Scaled Up**: Component separation and redundancy - enterprise-grade architecture

### Common Patterns
- **Load Balancing**: Distribute traffic across multiple servers
- **Database Replication**: Separate read and write operations
- **Monitoring**: Essential for maintaining system health
- **Security Layers**: Multiple levels of protection (defense in depth)
- **Component Separation**: Dedicated servers for specific functions
- **High Availability**: Eliminate single points of failure at every level

### Real-World Considerations
- **Scalability**: Plan for growth and traffic spikes
- **Security**: Protect against various attack vectors
- **Observability**: Monitor, log, and alert on system behavior
- **Redundancy**: Eliminate single points of failure
- **Component Architecture**: Separate concerns for better maintainability
- **Cost vs Performance**: Balance infrastructure costs with performance needs
- **Complexity Management**: Keep systems as simple as possible while meeting requirements