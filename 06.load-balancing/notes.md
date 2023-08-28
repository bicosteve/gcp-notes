# Load Balancing in Cloud

- cloud load balancer distributes user traffics across instances of an application in a single or multiple regions.
- it is a fully distributed, software defined managed service.
- Important features are;
  - health check - routes only to healthy instances thus assist in failure recoveries.
  - auto scaling - based on the number of incoming requests, it will automatically scale.
  - global load balancing with single anycast IP - this ip can be used to receive traffic from multiple regions across the world. Also supports internal load balancing.
- Load Balancing Enables;
  1. High availability even if one instance goes down, it should direct traffic to healthy ones.
  2. Auto Scaling.
  3. Resiliency.

### HTTP vs HTTPS vs TCP vs TLS vs UDP

- computes use protocols to communicate.
- there are multiple layers and multiple protocols.
- Network Layer - transfer bits and bytes
- Transport Layer - ensures the bits and bytes are transferred properly.
- Application Layer - Make REST API calls and Send Emails. It is at the top of the communication layers.
- Each layer makes use of the layers beneath it.
- Most applications talk at the application layer but some applications talk at network layer directly for high performance.

### Protocols used by each layer

1. Network Layer;
   - uses IP (Internet Protocol), transfers bytes however it is very unreliable since data might be lost
2. Transport Layer;
   - TCP (Transmission Protocol); Highly reliable over performance.
   - TLS (Transport Layer Security); It is secure since it encrypts the data being transferred.
   - UDP (User Datagram Protocol); It is an alternative to TCP and gives high performance over reliability.
3. Application Layer; - HTTP (Hypertext Transfer Protocol); it is stateless request response cycle. - HTTPS is secure version of the HTTP.
   Most applications communicate at application layer eg webservers. email servers, file transfers.
   All these applications use TCP/TLS at network layer for reliability.
   Applications that need high performance directly communcate at transport layer eg gaming applications, live video streaming use UDP and sacrifices reliability over performance.

#### Load Balancing in the cloud

- Load Balacing is on Network Services
- they are of different types i.e Application Load Balancer, Network Load Balancer,
- they can also be internet facing or internal single or multi-region
- UDP load balancer can only be in a single region
- Application Load Balancer (HTTP/S) is layer 7 load balancing for HTTP and HTTPS applications. Can be internet facing or internal single or multi region
- Network Load Balancer (TCP/SSL) is a layer 4 load balancing or proxy for applications that rely on TCP/SSL protocol. It is internet facing or internal single or multi-region.
- Network Load Balancing (UDP) is layer 4 load balancing for applications that rely on UDP protocol.
- Load Balancer can be created for internal vms or external applications
- For external applications, it has frontend, backed configurations and routing rules.

1. Backend Configurations: this is where to redirect traffic to eg managed instance groups
2. Frontend Configurations: where the traffic is received.
3. Routing Rules: configure rules how to redirect traffic to the backend.

#### Cloud Load Balancing Terms

1. Backend - group of endpoints that receive traffic from a GCP load balancer like instance groups.
2. Frontend - specify the ip address, port and protocol. This ip address is the frontend IP for your client requests.
3. Hosts and path rules - Defines rules redirecting the traffic to different backends.

   - based on path: bix.com/a vs bix.com/b
   - based on host: a.bix.com vs b.bix.com
   - http headers: authorization headers and methods POST,GET,etc

4. Load Balancing SSL/TSL Termination/Offloading

- Layer 7 is ssl termination
- Layer 4 is tls termination or offloading
- Client to Load Balancer; over the internet - https is recommended
- Load Balancer to VM instances through Google internal network HTTP is ok but HTTPS is preferred.
- SSL/TLS Termination/Offloading - client to load balancer: https/tls
- SSL/TLS Termination/Offloading - load balancer to VM instance: http/tcp

#### How to Choose Load Balancer

1. - Internal Trafffic
     For tcp or udp then use internal tcp/udp load balancer.
     For http/s traffic use internal http(s) load balancing.

2. - External Traffic
     for http/s traffic for a specific region, use regional http/s load balancer.
     for http/s traffic globally use global http/s load balancer.
     for tcp traffic with SSL offload use SSL proxy.
     for tcp traffic for global, use TCP proxy.
     for tcp traffic with client ip preserve go for External Network Load Balancing.

#### Load Balancing Features

- External http(s) is used for global external http or https and pass through proxy. Uses http port 80 or https 443
- Internal http(s) is used for regional internal http(s) traffic and pass through proxy. Use http port 80 or https port 443.
- SSL Proxy global, external, tcp with ssl offload uses pass through proxy wit a big list of destination ports.
- TCP Proxy is for global external, tcp without ssl offload and uses pass through with a big list of destination ports.
- Internal TCP/UDP load balancer, it is regional, internal, tcp and udp and uses pass through. Destination port is any.
- External TCP/UDP load balancer allows external regional, tcp, or udp traffic and uses pass through with any port destination.

#### Load Balancer Scenarios

1. You want only healthy instances to receive traffic - configure health check.
2. You want high availability for your VM instances - create mutliple MIGs for your VM instances in multiple regions. Load Balance using a load balancer.
3. You want to route requests to mutliple microservices using the same load balancer - create individual migs and backends for each micro service. Create host and path rules to redirect to specific microservice backend based on the path (/microservice-a,/microservice-b etc). You can route to Cloud Storage bucket as well.
4. You want to load balance global external https traffic across backend instances, across multiple regions - choose external http(s) load balancer.
5. You want SSL termination gor global non-https traffic with load balancing - choose SSL Proxy Load Balancer.
