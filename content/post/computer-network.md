---
title: "Computer Network"
author: "Kunyi Lu"
date: 2019-12-21T21:08:35-08:00
tags: ["Computer Network"]
draft: false
---

- - -
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

> **Terminology**: a network link</br>
> BDP: Bandwidth-Delay Product: number of bits "in flight" at any time(Bandwidth * Propagation delay)

3. queuing delay: how long does a packet have to sit in a buffer before it is processed? 
    - due to traffic mix and switch internals
4. processing delay: how long does the switch take to process a packet? 
    - due to traffic mix and switch internals

> **Terminology**: Round Trip Time(RTT)</br>
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

> **Terminology**: Protocols
> 1. Communication between peer layers on different systems is defined by protocols
> 2. An agreement between parties on how to communication
> 3. Defines the **syntax** and **semantics** of communication
> 4. Protocols exist at many levels</br>
    *L7 Application*: SMTP, HTTP, DNS, NTP</br>
    *L4 Transport*: TCP, UDP</br>
    *L3 Network*: IP</br>
    *L2 Data link*: Ethernet, FDDI, PPP</br>
    *L1 Physical*: Optical, Copper, Radio, PSTN</br>
    Note: IP is the narrow waist of the layering hourglass

# HTTP and the Web
## Web components
### Infrastructure
- clients
- servers(DNS, CDN, Datacenters)
### Content
- URL: naming content

> URL: Uniform Record Locator</br>
> protocol: //host-name[:port]/directory-path/resource
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

