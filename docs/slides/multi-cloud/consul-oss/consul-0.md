name: Consul-OSS-Workshop
class: title
count: false
![:scale 30%](images/consul_logo.svg)
.titletext[
Consul OSS Workshop]
Connect and Secure Services across any platform.

???
Welcome to our OSS Workshop for Consul.


Before we get started lets address a couple house keeping issues.

As Alli said...

Mute your mic during the presentation
We will go into smaller breakout rooms and encourage you to use your video & audio here

Please Post Questions in Chat and our HC Engineers will be available to help.
I'll be reviewing these between sessions as well.

This content is suitable for a 3-4 hour workshop.

---
name: Link-to-Slide-Deck
The Slide Deck
-------------------------
<br><br><br>
.center[
Follow along on your own computer at this link:

### [https://hashicorp.github.io/field-workshops-consul/slides/multi-cloud/consul-oss](https://hashicorp.github.io/field-workshops-consul/slides/multi-cloud/consul-oss)
]

???
These slides are published using the RemarkJS framework and Github Pages. View the source code for both the slide deck and the Instruqt labs here: https://www.github.com/hashicorp/field-workshops-consul. You may need to be invited to get access to this private code repo.

## Make Custom Edits
You can easily test a local copy of the slide deck with this python one-liner:
python -m SimpleHTTPServer

---
name: Introductions
Introductions
-------------------------

.contents[ ]
* Patrick Presto
* Sr. Solutions Engineer

???
I've worn many hats within operations

Linux Admin

Java Application Support

Developed deployment tools for weblogic, Jboss, tomcat, and .NET

Since early in my career I've been bridging the gap between Engineering and Operations working together to speed up application delivery 

In my previous role I was the director of a global infrastructure team.

---
name: Table-of-Contents
Table of Contents
=========================

1. Consul Overview
2. Consul Architecture
3. Consul Basics
    * Lab - Meet Consul
4. Consul Use Cases
5. Service Discovery
    * Lab - Service Discovery with Consul
6. Service Segmentation
    * Lab - Service Mesh with Consul
    * Bonus Lab - Service Mesh with Consul on K8s
    * Bonus Lab - Service Mesh with Consul Mesh Gateways

???
For today's session we are going to be covering the following topics.  
We are going to do an overview of why you even need consul.  Then we will go over how consul works from an architecture perspective.  We will then dive into some of the use cases that you would use consul for.  After that we will get our hands dirty with some labs starting with getting comfortable interacting with consul, then moving to service discovery and finally looking at consul connect and service segmentation.  Any questions?  Ok lets go!!
