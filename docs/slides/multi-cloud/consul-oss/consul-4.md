name: Chapter-4
class: title
# Chapter 4
## Consul Use Cases

---
name: Consul-Use-Cases
Consul Adoption Journey
-------------------------
.center[![:scale 70%](images/use_cases.png)]

???

Service Discovery & Service Mesh are the two big use cases we will focus on during this workshop.
As we start to look at various consul use cases...  Ask yourself, how you would go about adopting this in your environment.  

The best place to start is with **service discovery**.  
Getting this right is critical.  Its the core dataset you will use to build your network automation with.  

A good service discovery solution will discover services across all clouds public & private. all platforms bm,vm,container,servless or services running within some type of job scheduler like nomad or k8s.  This solution should discover services in near real time giving you the whole picture of your current eco system.  

A robust service discovery solution is the key to an successful service mesh.  

Without it you will spend a lot of your time addressing inconsistancies, automation gaps, and edge cases.
So we will start there.
---
name: Load-Balancers-Service-Discovery
class: compact
Service Discovery and Load Balancers
-------------------------

.center[![:scale 45%](images/consul-service-discovery.001.png)]

* Services location is paramount
* Traditionally done with load balancers
   * Expensive
   * Hard to maintain
   * Load grows as you scale
   * Requires health probes for every backend system
???
 
This is a model many of us are familiar with.  
If I want to add a new service like Order-History
I would subnimt a ticket to the network team asking 

**LB VIP,  Pool,  DNS,  Healthcheck**  

This gives me both HA and scalability, but its time consuming, error prone, typically has long lead times.  

Not to mention, LB are expensive, double everything for HA, and the more your services grow the more LB you need.

Consul can solve this.

---
name: Service-Discovery-with-Consul
class: compact
Service Discovery with Consul
-------------------------
.center[![:scale 60%](images/consul-service-discovery.002.png)]
* Services self-register
* Service health is defined by the service and maintained by the consul agent
* Services are able to query each other via DNS or HTTP

???
Lets take a look at the same workflow with Consul.
In a consul environment services are able to self register along with their unique health check requirements.  This makes it easy to define a healthy service.  

This coupled with consul using gossip service routing and healthchecking makes responses near real time.  

Consul allows for service discovery to be offloaded from the network and load balancer teams to the application deployment pipeline.  

This is a critical first step for any organization that wants to take advantage of the benefits of a service mesh.

* Example Transaction


---
name: Myriad-Use-Cases
class: compact
Solve Network Problems with Service Discovery
-------------------------
Consul prepared queries allow you to build logic into your DNS based service catalog. This enables transparent failover when the primary datacenter becomes unavailable.

```json
{
  "Name": "banking-app",
  "Service": {
    "Service": "banking-app",
    "Tags": ["v1.2.3"],
    "Failover": {
      "Datacenters": ["dc2", "dc3"]
    }
  }
}
```

???
prepared query can be use to automatically route traffic across DC. 

For example, If the banking-app goes down in DC2, and services in DC2 make a request to the banking-app it would normally fail. With a prepared query consul will see there are no healthy services locally and automatically routed the request to DC3 if there are healthy instances available.  

Making Failover transparent!

prepared queries are really cool.  And this is just 1 use case.

---
name: Myriad-Use-Cases-Example
Example
-------------------------

There are many other practical use cases that can be solved with the Consul. Some of these scenarios include: automatic routing of traffic to healthy nodes, blue/green deployments, **service locks, configuration management, watches, and more**. Learn more about practical, real-world uses for Consul in this HashiConf talk:

.center[
<a href="https://www.youtube.com/watch?v=XZZDVUCCilM" target=_blank>Consul Infrastructure Recipes - the story of Taco Hub 🌮</a>
]


???
* Service Locks
* Configuration Management
* Watches
* K/V Store

---
name: Secure-Networking-is-Hard
class: compact
Secure Networking is Hard
-------------------------
.center[![:scale 50%](images/consul-service-discovery.003.png)]

* Once applications can find each other security becomes the next concern
* This is usually done with a heavy dose of firewalls
* This adds significant burden to the network organization
* Huge lists of firewall rules

???
Now that service discovery is solved, our applications can find each other and now we need to secure them.  

Process Workflow: **App-cloud-network-security-network-cloud-app : 2-8 weeks**

Once again the over burdened network team has to create ip based firewall rules.  These rules can grow exponentially as your applications or microservices grow.   

---
name: Firewalls-Wont-Scale
Firewalls Won't Scale
-------------------------
.center[![:scale 70%](images/consul-service-discovery.004.png)]
* Heavy interdependencies
* Hard to automate
* Hard to optimize

???
If you take this mindset to its logical conclusion you will end up with something like this. 

Firewalls at every service trying to maintain all the up and downstream communications channels. 

At scale this is completely unmanageable. 

---
name: Consul-Service-Mesh
Consul Connect - A Modern Service Mesh
-------------------------
.center[![:scale 80%](images/consul-service-discovery.005.png)]

???
Using consul connect in conjunction with a proxy (like Envoy) will allow for several things.  

Consul can now provide all services their own Cert that is not IP based.  
Using this cert the service has an **identity that is tied to the service no matter where its running**.
* Consul also uses this Cert to Automatically enable mTLS connections between services for you.
* You can now define intentions or rules that say what service is allowed to talk to what service.  
* All Certificates and Proxy settings are automatically managed by consul (3rd party PKI manage is supported).

The power of this is that all of this can be defined in a simple service definition.

---
name: Consul-Service-Definition
class: compact
Consul Service Definition
-------------------------

```hcl
services {
  name = “web-app"
  port = 9090
  connect {
    sidecar_service {
      port = 20000
      proxy {
        local_service_address = "127.0.0.1"
        local_service_port = 9090
        upstreams {
          destination_name = “order-processing”
          local_bind_port = 8003
        }
      }
    }
  }
}
```

???
As you can see in this example all the connection definition is simply defined as a part of the service definition.  

---
name: How-do-we-secure-this
How do we secure this?
-------------------------
.center[![:scale 70%](images/consul-service-discovery.006.png)]
.center[With great power comes great responsibility...🕸️]

???
Now at scale inside a service mesh there might start to be some issue with all these connection zipping around between datacenters and clouds.  

Its difficult to maintain good network edge security when you have a wide berth of communication happening even if the port range is well defined.  

---
name: Mesh-Gateways
Consul Mesh Gateways
-------------------------
.center[![:scale 70%](images/consul-service-discovery.007.png)]
.center[Secure connections between any app or service across disparate environments]

???
We solve this problem using a Mesh Gateway.  

These Gateways allow a single point at the network edge that all mesh traffic flows over.  

So the network teams can control the ingress/egress points at the edge of the network 

while still allowing the app teams the flexibility to run application components on the platform of their choosing.  
