# Web Infrastructure Design

This project tracks the evolution of a web stack from a basic single-server setup to a highly available, secured, and scalable architecture.

### 0. Simple Web Stack
A foundational setup where all components live on one machine.
•	How it works: A user types www.foobar.com into their browser. The DNS translates this name to the IP 8.8.8.8. The browser communicates with the server via HTTP.
•	The Components:
•	Server: The "computer" hosting the files.
•	Web Server (Nginx): Accepts requests and serves static content.
•	App Server: Runs the code logic.
•	Database (MySQL): Stores data.
•	Issues:
•	SPOF: If the server fails, everything is offline.
•	Maintenance: Deploying new code often requires a restart, causing downtime.
•	Scalability: Cannot handle high traffic spikes.


### 1. Distributed Web Infrastructure
A three-server setup designed to handle more traffic and provide redundancy.
•	Why Additions? * Load Balancer (HAproxy): Distributes traffic between two servers so neither is overwhelmed.
•	Round Robin: An algorithm that sends requests to each server in turns (A then B then A...).
•	Database Cluster: Uses a Primary-Replica setup.
•	Primary: Handles data changes (Writes).
•	Replica: Syncs from the Primary and handles data requests (Reads).
•	Issues: Still lacks security (no HTTPS/Firewall) and has no monitoring to alert us when things break.

### 2. Secured and Monitored Web Infrastructure
Making the infrastructure professional, safe, and observable.
•	Firewalls: Act as security guards, blocking unauthorized traffic.
•	HTTPS/SSL: Encrypts the data sent between the user and the server so it cannot be stolen.
•	Monitoring (e.g., Sumologic): * Why: To catch errors and performance issues before users do.
•	QPS: We monitor "Queries Per Second" by analyzing web server logs.
•	Issues: * SSL Termination: If encryption stops at the load balancer, traffic is "naked" (unencrypted) inside the internal network.
•	Resource Contention: Since all components (DB, Web, App) share one server, they compete for memory and CPU.

### 3. Scale Up
Decoupling the stack to allow for maximum growth and reliability.
•	Load Balancer Cluster: We add a second HAproxy (Active-Passive). If the main one dies, the second one wakes up immediately.
•	Split Tiers:
•	Web Tier: Dedicated Nginx servers.
•	App Tier: Dedicated Application servers.
•	Database Tier: Dedicated MySQL servers.
•	The Benefit: If your database is slow, you can upgrade just the database server without touching the web or application layers. This is the standard for modern, high-traffic applications.



Author: Amaal AlOtaibi
