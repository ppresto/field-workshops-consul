name: Chapter-2
class: title
# Chapter 2
## HashiCorp Consul Architecture

---
name: Introduction-to-Consul
Introduction to Consul
-------------------------
.center[![:scale 45%](images/multi-datacenter-federation.png)]

???
Consul is a complex system that has many different moving parts. To help users and developers of Consul form a mental model of how it works, this page documents the system architecture.

A Consul Cluster in a low latency network is refered to as a data center.  
* 10 ms or less.  ex: across AZ in same region.

WAN traffic might be across Regions and be around 100ms.
---
name: Introduction-to-Consul-Overview
class: img-right
Introduction to Consul - Overview
-------------------------
.center[![:scale 100%](images/multi-datacenter-federation.png)]

* A consul cluster is referred to as a datacenter
* A consul datacenter is made up of server nodes and client nodes
* 3 or 5 server nodes per datacenter
* 100s - 10,000s of client nodes

???
Within each datacenter we recommend three to five servers depending on your risk tolerance.

No limit to the number of clients, and they can easily scale into the tens of thousands

* client can run on k8s worker node and service all pods for efficiency.
---
name: Introduction-to-Consul-Gossip
class: img-right
Introduction to Consul - Gossip
-------------------------
.center[![:scale 100%](images/multi-datacenter-federation.png)]

* All agent communication is done via the Gossip Protocol
* Automatic configuration and datacenter discovery for Consul agents
* Agent failures is done at the collective agent level not at the server level
* Using Gossip this allows high scalability vs. traditional heartbeat schemes
* Node failure can be inferred by an agent failure

???
Each Consul datacenter operates in a LAN gossip pool containing all members of the datacenter, both clients and servers. 
This Gossip pool is used for a few purposes. 
1. Allows clients to automatically discover servers. 
2. Allows for reliable and fast event broadcasts.
3. Agent failure detection is shared by all members of the datacenter instead of being concentrated on just the consul servers.

traditional healthchecking are 1/1 heartbeat checks. Doesn't scale.  
* Gossip protocol uses Serf and scales logramithically (show scale)
https://ppresto.github.io/field-workshops-consul/slides/multi-cloud/consul-oss/#75

---
name: Introduction-to-Consul-Consensus
class: img-right
Introduction to Consul - Consensus
-------------------------
.center[![:scale 100%](images/multi-datacenter-federation.png)]

* Every Consul datacenter has a group of server nodes that work together to manage connected agents
* Using Raft the server nodes elect a leader
* A leader is responsible for processing all queries and has write authority to the KV store
* It is also responsible for transaction replication
* All requests to the server nodes are routed to the leader

???
How does the consul server cluster communicate with each other?

This brings us to the Consensus protocol.

The servers in each datacenter are all part of a single Raft peer set. This means that they work together to elect a single leader. 

The leader is responsible for processing all queries and transactions. 

Replicating all data to all peers as part of the consensus protocol.

* when a non-leader server receives an RPC request, it forwards it to the cluster leader.

---
name: Introduction-to-Consul-Multi-DC
class: img-right
Introduction to Consul - Multi-DC
-------------------------
.center[![:scale 100%](images/multi-datacenter-federation.png)]

* Gossip over a WAN connection is also possible
* Allows for request from one datacenter to be forwarded to another
* This allows for service level DR
* This allows for geographical service request handling

???
How do we discover other datacenters and services? 
Consul Servers also operate as part of a WAN gossip pool. 

This pool is optimized for the higher latency of the internet. The purpose of this pool is to allow datacenters to discover each other in a low-touch manner. 

When a server receives a request for a different datacenter, it forwards it to the correct datacenter's consul cluster making cross-datacenter requests fast and reliable.

---
name: Introduction-to-Consul-Protocols
Introduction to Consul - Protocols
-------------------------
Now you have a high level understanding of Consul's two primary Protocols:

* Consensus
* Gossip

If you want to learn more these protocols, check out the appendix.
???
Consensus protocol managed by Raft

Gossip eventually consistent protocol that gets health of all nodes

---
name: Introduction-to-Gossip-Skeptical
Introduction to Consul - Skeptical ?
-------------------------
.center[![:scale 60%](images/mitchell_tweet.png)]
