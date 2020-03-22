---
title: "Computer Network"
author: "Kunyi Lu"
date: 2019-12-21T21:08:35-08:00
tags: ["Computer Network"]
draft: false
---

---

# Overview

In general, internet is transforming everything. It ties together different networks by IP protocal.

## Two ways to share switched network

1. circuit switching
   - resources **reserved** per connection
   - kinds of circuits: time division multiplexing and frequency division multiplexing
   - timing: circuit establishment -> data transfer -> circuit teardown
   - Pros
     - predictable performance
     - simple/fast switching(once circuit established)
   - Cons
     - complexity of circuit setup/teardown
     - inefficient when traffic is bursty(not constantly)
     - circuit setup adds delay
     - switch fails -> its circuits fails
2. packet switching
   - each packet contains destination. packets treated independently, **on-demand**
   - Pros
     - efficient use of network resources
     - simpler to implement
     - robust: can "route around trouble"
   - Cons
     - Unpredictable performance
     - requires buffer management and congestion control

## How do we evaluate a network?

### Performance metrics

#### Delay

1. transmission delay: how long does it take to push all the bits of a packet into a link?
   - due to link properties
   - transmission delay decreases as bandwidth increases
2. propagation delay: how long does it take to move one bit from one end of a link to the other?
   - due to link properties

    > a network link</br>
    > BDP: Bandwidth-Delay Product: number of bits "in flight" at any time(Bandwidth \* Propagation delay)

3. queuing delay: how long does a packet have to sit in a buffer before it is processed?
   - due to traffic mix and switch internals
4. processing delay: how long does the switch take to process a packet?
   - due to traffic mix and switch internals

    > Round Trip Time(RTT)</br>
    > Definition: Time for a packet to go from a source to a destination and to come back</br>
    > Measuring delay is hard from one end

#### Loss

Definition: What fraction of the packets sent to a destination are dropped?

#### Throughput

Definition: At what rate is the destination receiving data from the source?

# Protocol Layering

## OSI layers

OSI stands for Open Systems Interconnection model

- Developed by the ISO
- layers
  - L7 Application
  - L6 Presentation
  - L5 Session
  - L4 Transport
  - L3 Network
  - L2 Data link
  - L1 Physical
- Session and presentation layers are often implemented as part of the application layer
- Lower three layers implemented everywhere. Top two layers implemented only at hosts

> Protocols </br>
> 1. Communication between peer layers on different systems is defined by protocols <br/>
> 2. An agreement between parties on how to communication <br/>
> 3. Defines the **syntax** and **semantics** of communication <br/>
> 4. Protocols exist at many levels</br>

  > *L7 Application*: SMTP, HTTP, DNS, NTP</br>
  > *L4 Transport*: TCP, UDP</br>
  > *L3 Network*: IP</br>
  > *L2 Data link*: Ethernet, FDDI, PPP</br>
  > *L1 Physical*: Optical, Copper, Radio, PSTN</br>
  Note: IP is the narrow waist of the layering hourglass

# HTTP and the Web

## Web components

### Infrastructure

- clients
- servers(DNS, CDN, Datacenters)

### Content

- URL: naming content

  > URL: Uniform Record Locator</br>
  > protocol: //host-name[:port]/directory-path/resource </br>
  > - protocol: http, ftp, https, smtp, rtsp, ...
  > - hostname: DNS name, IP address(extend the idea of hierarchical hostnames to include anything in a file system)
  > - port: defaults to protocal's standard port(http: 80, https: 443)
  > - directory path: hierarchical, reflecting file system
  > - resource: indentifies the desired resource

- HTML: formatting content

### Protocols for exchanging information: HTTP(Hyper Text Transfer Protocal)

#### Attribute

- client-server architecture
  - client-to-server communication
    - HTTP request message: request line, request headers, body
    - request line: method(GET, POST, PUT, DELETE), resource, and protocol version
  - server-to-client communication
    - HTTP response message: status line, response headers, body
    - status line: protocol version, status code, status phrase
- synchronous request/reply protocol
  - Runs over TCP, Port 80
- stateless
  - each request-response treated **independently**
  - cookies store state
    - client-side state maintenance
    - can provide authentication
- ASCII format

## Performance goals

1. user
   - fast downloads
   - high availability
2. content provider
   - happy users(hence, above)
   - cost-effective infrastructure
3. network
   - avoid overload

### Solutions

1. improve networking protocols including HTTP, TCP...: (user)fast downloads
2. caching and replication: (user)fast downloads, (user)high availability, (network)avoid overload
3. exploit economies of scale, e.g. webhosting, CDNs, datacenters: (content provider)cost-effective infrastructure

## Object request response time

- 1 RTT for TCP setup
- 1 RTT for HTTP request and first few bytes
- Transmission time

Total = 2RTT + Transmission

## Type of connections

1. non-persistent connections
   - default in HTTP 1.0
   - 2RTT + delta for each object in the HTML file
   - inefficient since we close the connection after each request
2. persistent connections
   - default in HTTP 1.1
   - maintain TCP connection across multiple requests
3. pipelined requests and responses
   - multiple requests can be contained in one TCP segment
4. parallel/concurrent connections

We can use persistent and pipelined connections together to improve efficiency.

## Caching

1. HOW
   - modifier to GET requests: If-modified-since
   - response header
     - expires: how long it's safe to cache the resource
     - no-cache: ignore all caches
2. WHERE
   - reverse proxies: cache documents close to server
     - proxy for server
     - client cannot see which server it makes request to
   - forward proxies: cache documents close to client
     - proxy for client
     - server cannot see which client makes the request

# DNS and CDN

## CDN(Content Distribution Networks)

- caching and replication as a service
  - caching: direct result of clients's request
  - replication: expectation of high access rate
- large-scale distributed storage infrastructure administered by one entity

## DNS(Domain Name System)

  > **Terminology** <br/>
  > machine addresses: e.g. 141.212.113.143 <br/>
  > machine names: e.g. cse.umich.edu <br/>
  > DNS: a directory service which tells us how we map from one to the other

### Goals

- uniqueness: no naming conflicts
- scalable
- distributed, autonomous administration
  - ability to update own machines' names
  - don't have to track everybody's updates
- highly available
- lookups are fast

### Hierarchy

#### Hierarchical namespace

- domains are subtrees: e.g. .edu, umich.edu, eecs.umich.edu
- name is **leaf-to-root** path
- depth of tree is arbitrary

#### Hierarchical administration

- zone: an administrative authority that is responsible for that portion of the hierarchy, e.g. Umich controls names: _.umich.edu, EECS controls names: _.eecs.umich.edu

#### Server hierarchy

- top of hierarchy: root servers
- next level: Top-level domain(TLD) servers
  - .com, .edu, etc.
  - managed professionally
- bottom level: Authoritative DNS servers
  - actually store the name-to-address mapping(stores "resource records" for all DNS names in the domain that it has authority for)
  - maintained by the corresponding administrative authority
  - every server knows the root, root server knows about all top-level domains

### DNS records

- DNS servers store resource records(RRs)
- Types
  - Type=A:(Address)
    - name = hostname
    - value = IP address
  - Type=NS:(Name Server)
    - name = domain
    - value = name of DNS server for domain
  - Type=CNAME:(Canonical Name)
    - name = alias name for some "canonical" name
    - value = canonical name
  - Type=MX:(Mail eXchanger)
    - name = domain in email address
    - value = name(s) of mail server(s)

### Two ways to resolve a name

- recursive name resolution: ask server to do it for you
- iterative name resolution: ask server who to ask next
- a mix of both

### DNS provides indirection

- addresses can change underneath
- name could map to multiple IP addresses
  - load balancing(CDN)
  - reducing latency by picking nearby servers(CDN)
- multiple names for the same address
  - many services(mail, www) on same machine
  - aliases like www.cnn.com and cnn.com
- the flexibility applies only within domain

# Video Streaming and Cloud Systems

## Video streaming

### Video medium

- often too large to send in one GET
- compression is key
  - lots of algorithms to compress

### HTTP streaming

- video is stored at an HTTP server with a URL
- clients send a GET request for the URL
- server sends the video file as a stream
- client first buffers for a while
- once the buffer reaches a threshold, the video plays in the foreground and more frames are downloaded in the background
- same bitrate for all clients. cannot dynamically adapt to conditions
  - Solution: DASH: Dynamic Adaptive Streaming over HTTP

## Datacenters

> Datacenter networks </br>
> Tens to hundreds of thousands of hosts, often closely coupled, in close proximity </br>
> Core -> Aggregation -> Rack

- forms the backend of modern web services
- partition-aggregate traffic
- less than 200 milliseconds between receiving user query in the browser and displaying the results

# Transport Layer



# TCP Basics

# Flow and Congestion Control

# More Congestion Control

# Network Layer and IP

# IP Routers