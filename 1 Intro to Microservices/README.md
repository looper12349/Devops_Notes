Microservice Architecture — Notes
1. 🧱 What is Microservice Architecture?

Microservice architecture is a software design pattern where an application is divided into a collection of small, independent services that communicate over APIs.

🔹 Key Features:

Each service is independently deployable and scalable.

Services communicate typically via HTTP (REST), gRPC, or message queues.

Each microservice owns its own database.

Enables continuous delivery and fault isolation.

2. 🌐 DNS Server & IP Connection
🔸 Step-by-step:

User enters a domain name (e.g., example.com).

The DNS resolver sends a query to find the corresponding IP address.

The DNS hierarchy is followed:

Local cache → Root DNS → TLD DNS → Authoritative DNS.

Once the IP address is found, it is returned to the client.

The browser or service then connects to the server using that IP via TCP/UDP.

🔸 DNS in Microservices:

Services often use service discovery (like Consul, Eureka, or Kubernetes DNS) to dynamically resolve service IPs.

Example: user-service.default.svc.cluster.local (Kubernetes internal DNS).

3. ⚖️ Load Balancers
🔸 What is a Load Balancer?

A load balancer distributes incoming network or application traffic across multiple servers to ensure:

High availability,

Reliability,

Better performance,

No single point of failure.

🔹 Types of Load Balancers:
1. Layer 4 (Transport Layer)

Works at the TCP/UDP level.

Distributes traffic based on IP address and port.

Example: AWS NLB, HAProxy (L4 mode).

2. Layer 7 (Application Layer)

Works at the HTTP/HTTPS level.

Can route based on URL, header, cookie, etc.

Example: AWS ALB, Nginx, Traefik.

3. Hardware Load Balancers

Physical devices used in data centers (e.g., F5, Citrix ADC).

4. Software Load Balancers

Software-based solutions (e.g., Nginx, HAProxy, Envoy).

🔹 Load Balancing Algorithms:

Round Robin

Least Connections

IP Hash

Weighted Round Robin

Random

4. 🔐 Authentication vs Authorization
Concept	Definition	Example
Authentication	Verifying who the user is.	Login using username/password.
Authorization	Determining what the user can do.	Admins can delete posts; users cannot.
🔹 Common Methods:

OAuth 2.0

JWT (JSON Web Token)

SAML

Basic Authentication

API Keys

5. 🔁 Sync vs Async Communication
Type	Description	Example	Use Case
Synchronous	Request waits for a response before proceeding.	REST API call	Payment processing
Asynchronous	Request doesn’t wait for response; processes in background.	Message queues (RabbitMQ, Kafka)	Email sending, logging
🔹 Key Difference:

Sync = Blocking

Async = Non-blocking