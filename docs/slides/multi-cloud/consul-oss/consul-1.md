name: Chapter-1
class: title
# Chapter 1
## Introduction to Consul

---
name: HashiCorp-Consul-Overview
Consul Overview
-------------------------
.center[![:scale 10%](images/consul_logo.png)]

HashiCorp Consul is an API-driven service networking solution. It connects and secures all your runtime services across public or private clouds.

For additional descriptions or instructions that expand on this workshop, please see the docs, API guide, and learning site:
* https://www.consul.io/docs/
* https://www.consul.io/api/
* https://learn.hashicorp.com/consul/

???
What is Consul?
Hashicorp consul is an API driven service networking solution.  It discovers, connects, and secures all your runtime service across public or private clouds.
* Around since 2014
* Most widely deployed service discovery tool on AWS today.  
* As for service discovery and configuraiton mgmt it Won a huge part of market.
  
What is the most widely downloaded Hashicorp tool today?
* Many think terraform and vault
* Its actually consul.
**So there is massive adoption!**  

These are some public resources available to you for learning.  If you dont know about these check them out.  They are out there for you and include many working examples to expediate your working knowledge of service discovery and service mesh

---
name: The-Shift
The shift from static to dynamic
-------------------------
.center[![:scale 50%](images/static_to_dynamic.png)]
.center[Physical servers, to VMs, to containers...]

As our applications have shifted from monoliths to microservices, the networking landscape has changed drastically. Let's briefly explore the history of this shift, and how Consul can help us with its challenges.

???
Static to Dynamic Shift

We are moving away from the traditional DC [ clearly defined net, all ingress/egr -> fw] fw using IP address for identiy.

As we transition into modern/dyn DC [ 3rd party net, dont know neighbor, no control on IP, IP often ephemeral]

We're shifting away from the notion of a host. It used to be that this machine is a web server, this machine is an API server or a database. THis doesn't work in the cloud where IP's are dyn & short lived.  Now we think in terms of services.

I don't really care what's running on that machine. I have hundreds of machines as part of my Nomad cluster. I don't really know what's running where. What I care about is routing the payment api service to the billing  service to the database service.

Some of these services might not even exist until they get invoked. If I'm running on a Lambda endpoint, there is no host. Until the invocation happens, I don't even know an IP. 

It's a different way of thinking, How do I route to a service as opposed to how do I route to a host?

Lets take a quick look at the history of the DC to better understand

---
name: Client-Server
class: img-right
Introduction of Client & Server
-------------------------
.center[![:scale 100%](images/client_server_flow.png)]

<br><br>
* Single application per Server
* No app mobility
* Security mapped to IP
* Seldom horizontal scale of an app
* High trust zones and perimeter

???
In the 1990’s we had a client / server model.

Some of us remember deploying applications in this model
* one app per server
* apps were identified by their host or IP 
* put this in spreadsheets to track inventory
* wrap firewall rules around it and it worked fine

We weren’t really supporting API’s on mobile phones or other partners coming to our network


---
name: Introduction-of-VMs
class: img-right
Introduction of the VM
-------------------------
.center[![:scale 100%](images/vm_flow.png)]

<br><br>
* Better HW utilization
* Basic networking in Hypervisor
* VM mobility
* Some Horizontal scaling
* Load balancers
* Spanning trees

???
Fast forward a little bit and we started to introduce virtualization giving us a layer of abstration that allowed for
* Better h/w utilization  
* application scalability
* larger use of load balancers

More things for infrastructure to manage
---
name: Introduction-of-the-Fabric
class: img-right
Introduction of the Fabric
-------------------------
.center[![:scale 100%](images/fabric_flow.png)]

<br><br>
* L2 Fabrics
* Mostly proprietary L2 routing
* More single service instances
* More Load Balancers
* Spine & Leaf

???
Things grow and multiply over time.  

So we created smarter switches that register endpoints, connect regional domains.  These lead to spine/leaf architecture that enabled us to further scale and increased # of load balancers.
...

---
name: Introduction-of-the-Microservice
class: img-right
Introduction of the Microservice
-------------------------
.center[![:scale 100%](images/microservices.png)]

<br><br>
* Highly maintainable and testable
* Loosely coupled
* Independently deployable
* Organized around business capabilities
* Owned by a small team

???

Somewhere between Service Fabrics and Microservices 

We lost our financial service companies and 

Got tech companies that do financial services.


---
name: Introduction-of-the-SDN
class: img-right
Introduction of the SDN
-------------------------
.center[![:scale 100%](images/sdn_flow.png)]

<br><br>
* Network automation
* Self-Service
* Separation of duties - Who operates SDN?
* Lower visibility for network admin

???
Fast forward a little and SDN's are introduced to help push application delivery faster.  We see a need for networking at the software level

SDN:
Traditional Network vendors are throwing SDN’s as an answer and we haven’t really seen a lot of ROI yet.  But its about the concept.  

What is a SDN?    
Dev’s want self services, and there’s a lot of roles/responsibilities b/w dev, network, security, ops teams.  Like who owns what in this world?  Another people process and tools thing.  Theres a lot of resistance to change because there’s a lower level of visibility for the networking admin in this world so theres some resistance.

---
name: Introduction-of-the-Multi-Cloud-Hybrid
class: img-right
Introduction of Multi-Cloud - Hybrid
-------------------------
.center[![:scale 100%](images/hybrid_cloud_flow.png)]

<br><br>
* Where is my app instance?

???
And Now where in the middle of this transformation 
* from onprem to multi-cloud
* and even multi-platform (asg,container,serverless) 
* where is my app instance?  

Maybe its in a private 10.x network trying to talk to somethng onprem in the same 10.x network space and its pre-defined you can't even change it.

---
name: Introduction-of-the-Multi-Cloud-K8s
class: img-right
Introduction of Multi-Cloud - K8s
-------------------------
.center[![:scale 100%](images/hybrid_k8s_flow.png)]

<br><br>
* K8s src IP
* K8s networking - NAT / Calico / Flannel
* Access to K8S service - K8S Ingress et al

???
and throw in K8s and its even more confusing.  Maybe you have your SDN stuff on left maybe thats your openshift on some type of ingress solution, possibly some cloud container solution on right and in this world IP's are almost meaningless.  It just doesn't work and its where everyone is at today.

---
name: Introduction-Summary
Summary
-------------------------
.center[![:scale 50%](images/static_to_dynamic_flow.png)]
As you can see our networking model has drastically changed.
Let's learn a little more about how Consul works, and then we can revisit these challenges with Consul.

???
Summary:

Weve solved some issues with service discovery, but we are still trying to get to the end game.  How can we connect all these services together.

We are very much : Crawl - Walk - Run
Its a journey, we start out with service discovery.  70-80% of client workloads are related to things like getting F5 LB pool updates, or Nginx, HAProxy config updated.

Many new Greenfield deployments using cloud K8s services are using more advanced mesh capabilities.  Not being tied to a scheduler or orchestrator allow us to not be tied to that journey but the whole show.

---
name: Live-Demo
class: center,middle
Live Demo
=========================

???
Let's do a short demo to show you one of the use cases Consul can help you solve.
